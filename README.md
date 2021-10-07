# GDAL'in ECW ve diğer formatlar desteğinde derlenmesi 

## Neden derlenir?

Bir dosyadan kaynak kodu derlemek, bu dosyaları yürütülebilir bir dosyaya (.exe), dinamik yük kitaplığına (.dll) veya statik kitaplığa (.lib) bağlamak anlamına gelir.

dll :  dynamic-load library

Bu işlem aşağıdaki gibi olur:

C++ önişlemcisi, her kaynak dosyadaki tüm #directives ve makro tanımlarını dönüştürür. Bu bir çeviri birimi oluşturur.
C++ derleyicisi, ayarlanmış olan derleyici seçeneklerini uygulayarak her çeviri birimini nesne dosyalarında (.obj) derler. 
Bağlayıcı, ayarlanmış bağlayıcı seçeneklerini uygulayarak nesne dosyalarını tek bir yürütülebilir dosyada birleştirir.


birden çok platform (x86, x64, ARM vb.)


## Make dosyaları nedir?


## GDAL (Geospatial Data Abstraction Library) nedir?
GDAL (Jeo-uzamsal Veri Soyutlama Kitaplığı), raster ve vektör coğrafi verilerini işlemek için kullanılan bir araçtır. ArcGIS ve QGIS dahil olmak üzere birçok CBS yazılımında yardımcı araç olarak kullanılır. 
C++ programlama dilinde yazılmıştır.

## ECW (Enhanced Compressed Wavelets) nedir?
ECW, optimize edilmiş bir jeo-uzamsal raster dosya formatıdır.
ECW teknolojisi ile görüntüler istenilen oranlarda sıkıştırılabilir. Hem sıkıştırma hem de açma kütüphaneleri bulunan bir standarttır. 
Ayrıca ECW, görüntü piramidini kendisi oluşturur. Böylece çok büyük alanların çok
daha kısa sürede ekran üzerinde görüntülenmesi sağlanır.

ERDAS ECW/JP2 v4.x ve v5.x SDK yalnızca görüntü açma için ücretsizdir.  ECW 3.3 SDK için, 500 MB'tan küçük resimler ücretsiz olarak sıkıştırılabilirken, daha büyük resimler ERDAS'tan lisans alınmasını gerektirir. 

## Derleme için yapılan işlemler


İlk olarak kaynak dosyaları indirilir. <br>
GDAL için: (https://gdal.org/download.html#current-releases)[https://gdal.org/download.html#current-releases]<br>
PROJ için: (https://proj.org/install.html#windows)[https://proj.org/install.html#windows]<br>
OSGeo için: (https://www.osgeo.org/projects/osgeo4w/)[https://www.osgeo.org/projects/osgeo4w/]<br>
ECW için: (https://github.com/sasgis/libecwj2)[https://github.com/sasgis/libecwj2]

Daha sonra, GDAL'da bulunan nmake.opt dosyasında bazı değişiklikler yapılır.<br>
PROJ için include ve library path'i girilmelidir.<br>

PROJ_INCLUDE = -I(YOUR_PROJ_SRC_PATH)<br>
PROJ_LIBRARY = (YOUR_OSGeo4W64_Proj_lib_path)<br>

<!--
libecwj2-3.3 dosyasında bulunan Source/NCSBuildQmake altında libecwj2_win32_vc90.vcproj derlendi
Oluşan lib ve dll dosyaları Source altında Debug klasörüne eklendi.
-->
ECW format desteği eklenmek istenirse aynı PROJ'daki gibi path eklenir: <br>
ECWDIR  = 	C:\warmerda\libecwj2-3.3<br>
ECWLIB  = 	$(ECWDIR)\Source\NCSBuildQmake\Debug\libecwj2.lib<br>
ECWFLAGS = 	-DECWSDK_VERSION=33 \<br>
		    -I$(ECWDIR)\include -I$(ECWDIR)/Source/include \<br>


Visual Studio sistemini çağırmak için 'generate_vcxproj.bat' çalıştırılır. <br>
Parametreler şu şekildedir: <br>
1. Visual C++ versiyonu
2. Windows Platform
3. Base dosya adı

Örneğin: <br>
.\generate_vcxproj 14.0 64 gdal_vs2019

Sonucunda, <br>
gdal_vs2019.vcxproj <br>
gdal_vs2019.vcxproj.user <br>
gdal_vs2019.vcxproj.filters oluşur. 

Bir VCXPROJ dosyası, C ++ ile program geliştirmek için kullanılan bir Microsoft Visual Studio bileşeni olan Visual C ++ tarafından oluşturulan bir yazılım geliştirme projesidir.
Çalıştırılabilir programlar geliştirmek ve oluşturmak için kullanılır.





## Referanslar
* [GDAL](https://gdal.org/)
* [PROJ](https://proj.org/)
* [Hexagon](https://share.vidyard.com/watch/tYiQYjjhHx2ZVFatXKyTru?)
* [Oracle](https://www.oracle.com/technetwork/database/enterprise-edition/gdal-howto-compile-windows-128267.txt)
* [OSGEO ](https://trac.osgeo.org/gdal/wiki/ECW)
* [Software Using GDAL](https://gdal.org/software_using_gdal.html#software-using-gdal)
* [GDAL Raster Drivers](https://gdal.org/drivers/raster/index.html)
* [GIS Internals](https://www.gisinternals.com/)