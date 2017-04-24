

* [在github中找到该项目](https://github.com/tesseract-ocr/tesseract)

* [A .NET wrapper for tesseract-ocr](https://github.com/charlesw/tesseract)

* [Samples that demonstrate using Tesseract .Net Wrapper](https://github.com/charlesw/tesseract)


1.打开上面的第三个链接，点击Download Zip下载测试demo

   ![NewRepository]({{ site.github.url }}/images/Download_Tesseract_Sample.png)

2.创建项目，不要把项目名称取成Tesseract，因为和DLL名字一样不然会报错

	未能加载文件或程序集“Tesseract, Version=3.0.2.0, Culture=neutral, PublicKeyToken=ebeb3d86bef60cbe”或它的某一个依赖项。找到的程序集清单定义与程序集引用不匹配。 (异常来自 HRESULT:0x80131040)

3.从vs2015的Nuget包里安装，选择第一个Tesseract由Charles Weld，版本是Tesseract 3.0.2.0

   ![NewRepository]({{ site.github.url }}/images/Nuget_Tesseract.png)

4.从测试Demo复制tessdata语言文件和phototest.tif测试文件到程序目录下

   ![NewRepository]({{ site.github.url }}/images/Tessdata_Phototest.png)

5.新建app.config文件，最好在设置里把复制到输出目录改成:始终复制

	<system.diagnostics>
		<sources>
			<source name="Tesseract" switchValue="Verbose">
				<listeners>
					<clear />
					<add name="console" />
					<!-- Uncomment to log to file
					<add name="file" />
					-->
				</listeners>
			</source>
		</sources>
		<sharedListeners>
			<add name="console" type="System.Diagnostics.ConsoleTraceListener" />

			<!-- Uncomment to log to file
			<add name="file"
			   type="System.Diagnostics.TextWriterTraceListener"
			   initializeData="c:\log\tesseract.log" />
			-->
		</sharedListeners>
	</system.diagnostics>
	


6.简单的测试代码，注意路径，我用测试Demo里的链接格式试过是不行的，使用Debug和Release里的tessdata路径也不行，还不知道原因  
	Debug下我程序当前的目录为，D:\Documents\Visual Studio 2015\Projects\TesseractDemo\TesseractDemo\bin\Debug  
	路径设置为D:\Documents\Visual Studio 2015\Projects\TesseractDemo\TesseractDemo\tessdata	
	

	static void Main(string[] args)
	{
		Console.WriteLine(System.Environment.CurrentDirectory);
		var testImagePath = "./phototest.tif";
		using (var engine = new TesseractEngine("../../tessdata", "eng", EngineMode.Default))
		{
			using (var img = Pix.LoadFromFile(testImagePath))
			{
				using (var page = engine.Process(img))
				{
					var text = page.GetText();
					Console.Write(text);
				}
			}
		}
	}	