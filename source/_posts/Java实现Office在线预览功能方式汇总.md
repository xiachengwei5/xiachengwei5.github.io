---
title: Java实现Office在线预览功能方式汇总
data: 2018-01-28
comments: true
tags: [java, jacob, office转pdf, 在线预览]
---

文件的上传下载可以说是现在系统的标配功能，为了用户提升体验，一般对上传上来的常用Office文件（如：word/excel/ppt）都需要支持在线预览，而现在的浏览器都不支持直接打开这些Office文件。要实现在线预览就需要先通过后台程序将这些文件转换成统一的浏览器能直接打开的文件格式。
<!-- more -->

### 一、通过Java+Jacob的方式将Office转换成PDF

PDF(*Portable Document Format的简称，意为“便携式文档格式”*)是一种跨平台的文档格式，能通过浏览器直接打开并且不会出现格式错位和乱码等问题，同时Office文件都能友好的转换成PDF，所以为了实现在线预览功能首先想到的就是将Office文件先转换成PDF格式的文件，再通过浏览器直接打开进行预览。

1. **`jacob` 简介** 

   `jacob`（`java com bridge`，java com桥）分为两个部分，`jacob.jar` 和 `jacob.dll`，使用时两个东西的版本要一致，而且还分32位和64位，它的位数和jdk的位数有关，与操作系统的位数无关（这句貌似是废话，难道不是操作系统的位数决定了jdk的位数然后决定了.dll的位数吗）。它的原理是通过`java` 的 `JNI`  (`Java Native Interface` Java本地调用)功能，调用动态连接库(`.dll`)，通过这个com桥来操作com组件完成对office文档的操作，本质上是通过VBA(`Visual Basic for Applications` Visual Basic宏语言) 操作Office文件。

   其实早在1994年微软发行的Excel5.0版本中，即具备了VBA的宏功能，只是后来出现了[宏病毒](https://baike.baidu.com/item/%E5%AE%8F%E7%97%85%E6%AF%92/1568984?fr=aladdin) ，有一定的安全隐患导致office中默认宏功能是锁定的，需要手动设置才能看到，如下图：

   ![Microsoft 宏](http://olywxnzqu.bkt.clouddn.com/image/office-view-online/microsoft_word_hong.png) 

   ![WPS 宏](http://olywxnzqu.bkt.clouddn.com/image/office-view-online/wps_word_hong.png) 

   ​

   **优势：**

   > * 转换速度快：因为本质上是通过VBA操作Office文件，即通过office的"另存为PDF"功能进行操作，转换速度快，精确度高，不会出现格式错乱、乱码等问题；
   > * 兼容性强：目前已经能实现常用格式：doc/docx/xls/xlsx/ppt/pptx/wps/et/dps的转换；
   > * 扩展性强：因为本质上调用的是office原生的操作，转换过程中添加其他的功能也很简单，如：添加水印、插入图片、修改内容等；

   **劣势：** 

   > * 跨平台性较弱：`.dll` 文件不能在`Linux` 系统上运行导致整个功能不能在`Linux` 平台上使用；

   `Linux` 上不能安装*Microsoft office* 且不支持 `.dll` 文件，导致其不能实现跨平台使用，不过，根据对`jacob` 运行原理的研究，让其实现跨平台也不是不可能，这里可以开个脑洞，虽然`Linux` 上不能安装 *Microsoft office* 但是可以安装`WPS` （已经测试过在只安装WPS的环境下转换成PDF完全没有问题），虽然`Linux` 上不支持动态连接库(`.dll`)，但是可以支持其特有的动态链接库（`.so` Share Object）,那是不是只需要将编译生成`.dll` 的源码找来在`Linux` 下编译生成`.so` 文件就可以了呢。好，现在的问题就是去找编译生成`.dll` 的源码。


2. **环境搭建** 

`jacob` 的[官网](http://danadler.com/jacob/) （这是我找到的最像官网的网站）貌似已经停止运维了，能下载到的最新的`jar` 包还是2004年发布是1.7版本（貌似还不能用），而现在最新的版本是1.18。下载 [jacob.jar](https://sourceforge.net/projects/jacob-project/?source=typ_redirect) ，并在系统中添加依赖，将压缩包中的 `.dll` 文件放在 `%JAVA_HOME%\bin`（jdk\bin） 中。

**注意：** `.jar` 和 `.dll` 的版本要一致，同时`.dll` 的位数要与`jdk` 的位数相同，即需要配套使用。

3. **编写代码** 

``` java
package com.xia.java;

import com.jacob.activeX.ActiveXComponent;
import com.jacob.com.ComThread;
import com.jacob.com.Dispatch;
import com.jacob.com.Variant;

/**
 * 类描述： 提供统一的将office格式的文件转换成pdf格式的工具类
 * 创建人：Jeff 
 * 创建时间：2018年1月27日 下午1:35:31
 * @version 1.0
 */
public class Office2PdfByJacob {
	
	private static final int wdFormatPDF = 17; 				// word保存为pdf格式宏，值为17
	private static final int xlTypePDF = 0; 				// xlTypePDF为特定值0
	private static final int ppSaveAsPDF = 32; 				// ppSaveAsPDF为特定值32

	/**
	 * 将word(包含：doc, docx, wps格式)转换成pdf
	 * 
	 * @param srcFilePath   源文件路径
	 * @param pdfFilePath   生成PDF后存的路径
	 * @return boolean (转换成功返回true, 转换失败返回true)
	 */
	public static boolean word2Pdf(String srcFilePath, String pdfFilePath) {
		long startTime = System.currentTimeMillis();
		ActiveXComponent app = null;
		Dispatch doc = null;
		try {
			ComThread.InitSTA();
			app = new ActiveXComponent("Word.Application");
			app.setProperty("Visible", false);
			Dispatch docs = app.getProperty("Documents").toDispatch();
			doc = Dispatch.invoke(docs, "Open", Dispatch.Method,
					new Object[] { srcFilePath, new Variant(false), new Variant(true), // 是否只读
							new Variant(false), new Variant("pwd") },
					new int[1]).toDispatch();
			// Dispatch.put(doc, "Compatibility", false); //兼容性检查,为特定值false不正确
			Dispatch.put(doc, "RemovePersonalInformation", false);
			Dispatch.call(doc, "ExportAsFixedFormat", pdfFilePath, wdFormatPDF);
			long endTime = System.currentTimeMillis();
			System.out.println("转换耗时："+(endTime - startTime) / 1000 + "秒");
			return true;
		} catch (Exception e) {
			e.printStackTrace();
			System.out.println("转换失败");
			return false;
		} finally {
			if (doc != null) {
				Dispatch.call(doc, "Close", false);
			}
			if (app != null) {
				app.invoke("Quit", 0);
			}
			ComThread.Release();
		}
	}

	/**
	 * 将excel(包含：xls, xlsx, et格式)转换成pdf
	 * 
	 * @param srcFilePath   源文件路径
	 * @param pdfFilePath   生成PDF后存的路径
	 * @return boolean (转换成功返回true, 转换失败返回true)
	 */
	public static boolean excel2Pdf(String srcFilePath, String pdfFilePath) {
		long startTime = System.currentTimeMillis();
		ActiveXComponent app = null;
		Dispatch excel = null;
		try {
			ComThread.InitSTA();
			app = new ActiveXComponent("Excel.Application");
			app.setProperty("Visible", false);
			Dispatch excels = app.getProperty("Workbooks").toDispatch();
			excel = Dispatch.call(excels, "Open", srcFilePath, false, true).toDispatch();
			Dispatch.call(excel, "ExportAsFixedFormat", xlTypePDF, pdfFilePath);
			long endTime = System.currentTimeMillis();
			System.out.println("转换耗时："+(endTime - startTime) / 1000 + "秒");
			return true;
		} catch (Exception e) {
			e.printStackTrace();
			System.out.println("转换失败");
			return false;
		} finally {
			if (excel != null) {
				Dispatch.call(excel, "Close");
			}
			if (app != null) {
				app.invoke("Quit");
			}
			ComThread.Release();
		}
	}
	
	/**
	 * 将ppt(包含：ppt, pptx, dps格式)转换成pdf
	 * 
	 * @param srcFilePath   源文件路径
	 * @param pdfFilePath   生成PDF后存的路径
	 * @return boolean (转换成功返回true, 转换失败返回true)
	 */
	public static boolean ppt2Pdf(String srcFilePath, String pdfFilePath) {
		long startTime = System.currentTimeMillis();
		ActiveXComponent app = null;
		Dispatch ppt = null;
		try {
			ComThread.InitSTA();
			app = new ActiveXComponent("PowerPoint.Application");
			Dispatch ppts = app.getProperty("Presentations").toDispatch();

			ppt = Dispatch.call(ppts, "Open", srcFilePath, false, // ReadOnly
					true // Untitled指定文件是否有标题
			).toDispatch();
			Dispatch.call(ppt, "SaveAs", pdfFilePath, ppSaveAsPDF);
			long endTime = System.currentTimeMillis();
			System.out.println("转换耗时："+(endTime - startTime) / 1000 + "秒");
			return true; // set flag true;
		} catch (Exception e) {
			e.printStackTrace();
			System.out.println("转换失败");
			return false;
		} finally {
			if (ppt != null) {
				Dispatch.call(ppt, "Close");
			}
			if (app != null) {
				app.invoke("Quit");
			}
			ComThread.Release();
		}
	}
	
	public static void main(String[] args) {
		String srcFilePath = "E:\\file\\测试.pptx";
		String pdfFilePath = "E:\\file\\测试.pdf";
		//word2Pdf(srcFilePath, pdfFilePath);
		//excel2Pdf(srcFilePath, pdfFilePath);
		ppt2Pdf(srcFilePath, pdfFilePath);
	}
}
```

4. **参考资料** 

   [java将doc文件转换为pdf文件的三种方法](http://feifei.im/archives/93)

   [(java office转pdf) MS Office2010、WPS2015、OpenOffice4用Java将Office文档转换为PDF，WIN7 64位系统](http://blog.csdn.net/huitoukest/article/details/51374623) 

   [jacob操作office分享](http://men4661273.iteye.com/blog/2097871) 

   [JNI_最简单的Java调用C/C++代码](http://blog.csdn.net/wwj_748/article/details/28136061) 

   [linux下 java JNI调用C语言动态链接库](http://blog.csdn.net/alfredtofu/article/details/6861169) 

   [DLL转换so(NDK配置)](http://blog.csdn.net/panpen120/article/details/42193003) 

   [js学习笔记-PDF.js专题](http://blog.csdn.net/xiangcns/article/details/42089189) 

   [Linux平台Java调用so库-JNI使用例子](http://blog.csdn.net/u010212643/article/details/69567391) 

   [通过 JACOB 实现 Java 与 COM 组件的互操作](http://blog.csdn.net/dangerous_fire/article/details/61922656) 

### 二、通过Java+Jodconverter+Libreoffice的方式将Office转换成PDF 

通过Java+Jodconverter+Libreoffice的方式实现在服务器环境下（Linux系统）将office文件转换成pdf。

1. **简介** 

   `Libreoffice` 本身就自带通过命令将office文件转换成其他格式的功能，打开cmd，进入libreoffice安装目录/program，即可执行相关命令，将该目录配置到环境变量的path中即可全局使用：

   ``` shell
   # 打开libreoffice
   soffice
   # 查看帮助信息
   soffice -help
   # 文件格式转换提示如下
   soffice --convert-to pdf *.odt
   soffice --convert-to pdf xxx.doc
   soffice --convert-to epub *.doc
   soffice --convert-to pdf:writer_pdf_Export --outdir /home/user *.doc
   soffice --convert-to "html:XHTML Writer File:UTF8" *.doc
   soffice --convert-to "txt:Text (encoded):UTF8" *.doc
   ```

   调用jodconverter.jar的本质也就是调用这些libreoffice自带的命令；

   **优势：** 

   > * 能实现跨平台使用，兼容Windows和Linux操作系统；

   **劣势：** 

   > * 对office的兼容性不太好（如：当.doc/.wps格式的文件中有图片时转换成PDF会样式错乱，其实直接通过libreoffice打开.doc/.wps文件样式已经错乱了）；
   > * 转换较大文件（word 18M+、300页+）时会转换过程会卡死；

2. **环境搭建** 

   > * 在服务器上安装[Libreoffice](https://zh-cn.libreoffice.org/download/libreoffice-fresh/) ；
   > * 添加`jodconverter-core-3.0.1.jar`、`slf4j-api.jar`、`slf4j-nop.jar` 等jar包依赖，有些包可能没有显示用到，但是在其他jar包中有引用，也需要引入；
   > * 如果将office文件转换成pdf后乱码，很有可能是服务器上没有中文字体`simsun.ttc`  导致的，需要将windows下的字体拷贝到libreoffice的安装目录下`/opt/libreoffice6.0/share/fonts/truetype` 或拷贝到linux系统的字体`/usr/share/fonts` 目录下；

3. **编写代码** 

``` java
import org.apache.commons.io.FileUtils;
import org.artofsolving.jodconverter.OfficeDocumentConverter;  
import org.artofsolving.jodconverter.office.DefaultOfficeManagerConfiguration;  
import org.artofsolving.jodconverter.office.OfficeManager;    
import java.io.File;
import java.io.InputStream;
import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.IOException;
import java.util.regex.Pattern; 

public class Office2PdfByJodconverter {
	// 删除临时文件标识（false:不删除； true: 删除）
	private static boolean deleteFlag = false;
	
	public static void main(String[] args) {
		String sourcePath = "E:\\file\\2pdf\\NewFile.xml";
		String targetPath = "E:\\file\\2pdf\\NewFile.pdf";
		libreOffice2PDF(sourcePath,targetPath);
	}
	 
     /** 
     * 打开libreOffice服务的方法 
     * @return  
     */  
    public static String getLibreOfficeHome() {  
        String osName = System.getProperty("os.name");
        System.out.println("osName:"+osName);
        if (Pattern.matches("Linux.*", osName)) {  
            //获取linux系统下libreoffice主程序的位置
            //如果路径填写错误会报：officeHome must exit and be a dicrecory错误
            return "/opt/libreoffice6.0/";
        } else if (Pattern.matches("Windows.*", osName)) {  
            //获取windows系统下libreoffice主程序的位置  
            return "D:\\software\\LibreOffice";
        }  
        return null;  
    }  
    
    
    /** 
     * 转换libreoffice支持的文件为pdf 
     * @param inputfile 
     * @param outputfile 
     */  
    public static boolean libreOffice2PDF(String inputPath, String outputPath) {  
        long startTime = System.currentTimeMillis();
        // 根据文件转换成文件流
        File inputFilePath = new File(inputPath);
		File outputFilePath = new File(outputPath);
        // 获取当前系统安装libreoffice的路径
    	String LibreOffice_HOME = getLibreOfficeHome();
		String fileName = inputFilePath.getName();
		
		
        DefaultOfficeManagerConfiguration configuration = new DefaultOfficeManagerConfiguration();  
        // 设置libreOffice的安装目录
        configuration.setOfficeHome(new File(LibreOffice_HOME));  
        // 端口号  
        configuration.setPortNumber(8100);  
        // 设置任务执行超时为10分钟  
        configuration.setTaskExecutionTimeout(1000 * 60 * 25L);  
        // 设置任务队列超时为24小时  
        configuration.setTaskQueueTimeout(1000 * 60 * 60 * 24L);  

        OfficeManager officeManager = configuration.buildOfficeManager();  
        officeManager.start();  
        OfficeDocumentConverter converter = new OfficeDocumentConverter(officeManager);  
        converter.getFormatRegistry();  
        try {
        	// 如果是txt格式先对其进行转码
    		if(fileName.lastIndexOf(".") > 0) {
    			String fileSuffix =  fileName.substring(fileName.lastIndexOf(".")).toLowerCase();
    			if (".txt".equals(fileSuffix) || ".yml".equals(fileSuffix) || ".xml".equals(fileSuffix)) {  
    	            inputFilePath = TXTHandler(inputFilePath);
    	        }
    		} else {
    			System.out.println("该文件无后缀名，不能转换");
    			return false;
    		}
    		converter.convert(inputFilePath, outputFilePath);
            long endTime = System.currentTimeMillis();
    		System.out.println("转换耗时："+(endTime - startTime) / 1000 +"秒");  
            return true;
        } catch (Exception e) {  
            e.printStackTrace();  
            System.out.println("转换失败");  
            return false;
        } finally {  
            officeManager.stop();  
            if(deleteFlag) {
            	// 删除生成的临时文件
            	FileUtils.deleteQuietly(inputFilePath);
            }
        }  
    }  
    
    
    /** 
     * 转换txt文件编码的方法 
     * 在源文件的上直接转码会改变源文件的编码方式存在风险，有可能将源文件转换成乱码， 
     * 现在通过先创建临时文件转换完成后再删除临时文件的方式进行处理，保护源文件不受污染
     * @param file 
     * @return 
     */  
    public static File TXTHandler(File file) {  
    	File inputTempFile = null;
        //或gb2312  
        String code = "GBK";  
        byte[] head = new byte[3];
        try {  
            InputStream inputStream = new FileInputStream(file);  
            inputStream.read(head);  
            if (head[0] == -1 && head[1] == -2) {  
                code = "UTF-16";  
            } else if (head[0] == -2 && head[1] == -1) {  
                code = "Unicode";  
            } else if (head[0] == -17 && head[1] == -69 && head[2] == -65) {  
                code = "UTF-8";  
            }  
            inputStream.close(); 
            
            // 如果当前文件的编码格式是UTF-8则直接返回
            if (code.equals("UTF-8")) {  
                return file;  
            }
            
            // 根据获得的格式再重新读取文件，防止文件乱码
            String str = FileUtils.readFileToString(file, code);  
            
            // 将转码后的文件写成临时文件
            String currentTime = String.valueOf(System.currentTimeMillis());
            String filePath = file.getPath();
            // 如果此处写的不是绝对路而是相对路径则生成的文件在项目的根目录下
            String tempFilePath = filePath.substring(0, filePath.lastIndexOf("\\")+1)+currentTime+"-"+file.getName();
            
            inputTempFile = new File(tempFilePath);
            
            // 将按照文件编码格式读取的文件转换成UTF-8编码写到临时文件中
            FileUtils.writeStringToFile(inputTempFile, str, "UTF-8");
            // 修改删除临时文件的标识为：true，用来在转换完成之后删除临时文件
            deleteFlag = true;
        } catch (FileNotFoundException e) {  
            e.printStackTrace();  
        } catch (IOException e) {  
            e.printStackTrace();  
        }  
        return inputTempFile;
    } 
}
```



4. **参考资料** 

[Java使用libreoffice实现office文件转换成pdf格式，支持windows和linux](http://blog.csdn.net/hatherclare/article/details/79199951) 

[解决 程序报 SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder". 错误](https://www.cnblogs.com/FocusIN/p/5853009.html) 

[libreoffice转换文件为pdf文件乱码问题解决办法](http://www.cnblogs.com/heimirror/p/3792460.html) 


### 三、通过Java+poi的方式将Office转换成Html

测试完善中......





[java实现在线预览--poi实现word、excel、ppt转html](http://blog.csdn.net/chentao866/article/details/68065329) 

### 四、对纯文本格式文件转换编码的处理

浏览器是可以直接打开部分纯文本格式文件的，如：`txt/json/xml` 等，这类文件可以不用转换直接通过浏览器打开预览；还有部分纯文本格式文件浏览器可以直接打开但是浏览器默认的方式不是直接打开而是下载，如：`sql/yml` 等，这类文件可以通过匹配设置告诉浏览器通过文本打开，nginx设置如下：



但是这两种情况都存在一个共同的问题：**因编码方式不统一，会造成预览乱码** ；如果不是约定好的，要想解析纯文本文件就需要知道文件编码类型，由于文件编码类型众多（如`UTF-8，GBK，UTF-16,GB2312` 等），不根据文件原有编码类型进行解析就会出现乱码；所以在解析之前需要先获取纯文本文件的编码类型。

找了很多资料发现这块的编码转换还是有很多问题，不知道是获取的编码不够准确还是个编码之间有什么内在的联系，目前只能将本来就是`UTF-8`编码的纯文本文件转换成PDF不会出现乱码，其他编码格式的还没有完美解决，待后续研究透彻后再完善......



**参考资料** 

[[Java]获取txt文件编码格式](https://tieba.baidu.com/p/2089447448?red_tag=1407138954) 

[java如何获取文件编码格式](http://chengyue2007.iteye.com/blog/2043665) 

[Java获取文本文件字符编码的两种方法](http://blog.csdn.net/qq_29347295/article/details/78361796) 

[Java判断文本文件编码格式以及读取](http://blog.csdn.net/21aspnet/article/details/50612867) 