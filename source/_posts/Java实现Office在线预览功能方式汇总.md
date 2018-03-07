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



### 二、通过Java+poi的方式将Office转换成Html

测试完善中......



### 三、参考资料

[(java office转pdf) MS Office2010、WPS2015、OpenOffice4用Java将Office文档转换为PDF，WIN7 64位系统](http://blog.csdn.net/huitoukest/article/details/51374623) 

[jacob操作office分享](http://men4661273.iteye.com/blog/2097871) 

[JNI_最简单的Java调用C/C++代码](http://blog.csdn.net/wwj_748/article/details/28136061) 

[linux下 java JNI调用C语言动态链接库](http://blog.csdn.net/alfredtofu/article/details/6861169) 

[DLL转换so(NDK配置)](http://blog.csdn.net/panpen120/article/details/42193003) 

