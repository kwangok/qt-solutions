# qt-solutions 
>forked from qtproject/qt-solutions 



##change qt-solutions-master build win32 x64 version 
---
'''
cd qt-solutions-master
'''
 common.prj 
 '''
 exists(config.pri):infile(config.pri, SOLUTIONS_LIBRARY, yes): CONFIG += qtpropertybrowser-uselib
TEMPLATE += fakelib
QTPROPERTYBROWSER_LIBNAME = $$qtLibraryTarget(QtSolutions_PropertyBrowse)
TEMPLATE -= fakelib
QTPROPERTYBROWSER_LIBDIR = $$PWD/lib
unix:qtpropertybrowser-uselib:!qtpropertybrowser-buildlib:QMAKE_RPATHDIR += $$QTPROPERTYBROWSER_LIBDIR
'''
build.pro 
'''
TEMPLATE=lib
CONFIG += qt dll qtpropertybrowser-buildlib
mac:CONFIG += absolute_library_soname
win32|mac:!wince*:!win32-msvc:!macx-xcode:CONFIG += debug_and_release build_all
include(../src/qtpropertybrowser.pri)
TARGET = $$QTPROPERTYBROWSER_LIBNAME
DESTDIR = $$QTPROPERTYBROWSER_LIBDIR
win32 {
    DLLDESTDIR = $$[QT_INSTALL_BINS]
    QMAKE_DISTCLEAN += $$[QT_INSTALL_BINS]\\$${QTPROPERTYBROWSER_LIBNAME}.dll
	contains(QMAKE_HOST.arch, x86):{
		QMAKE_LFLAGS += /MACHINE:X86
	}

	contains(QMAKE_HOST.arch, x86_64):{
		QMAKE_CFLAGS           += /MD
		QMAKE_CXXFLAGS         += /MD
		DEFINES     += X64 __X64__ __x64__
		VCPROJ_ARCH = x64
		QMAKE_LFLAGS += /MACHINE:X64
	}	
}
target.path = $$DESTDIR
INSTALLS += target
'''

##use ms2015 
'''
"C:\Program Files (x86)\Microsoft Visual Studio 14.0\VC\vcvarsall.bat"  x64
F:\Qt5.15.0\msvc2015_64\bin\qmake.exe  -win32  -o Makefile D:\tools\qt-solutions-master\qt-solutions-master\qtpropertybrowser\qtpropertybrowser.pro
nmake release
'''
>note : LNK1158 copy rc.exe rc.dll to C:\Program Files (x86)\Microsoft Visual Studio 14.0\VC\bin\amd64 

