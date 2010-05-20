javadir = $(pkgdatadir)/lib

PKG = net/sourceforge/zbar
java_DATA = zbar.jar

java_LTLIBRARIES = libzbarjni.la
libzbarjni_la_CPPFLAGS = $(JAVA_CFLAGS) $(AM_CPPFLAGS)
libzbarjni_la_LIBADD = $(abs_top_builddir)/zbar/libzbar.la

libzbarjni_la_SOURCES = zbarjni.c zbarjni.h
BUILT_SOURCES = zbarjni.h
MAINTAINERCLEANFILES = zbarjni.h

zbar_jar_SRCS = \
    $(PKG)/Config.java $(PKG)/Orientation.java \
    $(PKG)/Symbol.java $(PKG)/SymbolIterator.java $(PKG)/SymbolSet.java \
    $(PKG)/Image.java $(PKG)/ImageScanner.java

zbar_jar_CLASSES = $(zbar_jar_SRCS:.java=.class) \
    $(PKG)/Symbol\$$Rect.class

test_SRCS = test/TestImage.java test/TestImageScanner.java \
    test/TestScanImage.java
test_CLASSES = TestImage TestImageScanner TestScanImage

CLEANFILES = zbar.jar $(zbar_jar_CLASSES) $(test_CLASSES:=.class)

zbarjni.h: $(zbar_jar_SRCS)
	$(MAKE) $(AM_MAKEFLAGS) zbar.jar
	classes=`echo $(zbar_jar_CLASSES:.class=) | tr / .` ; \
	    $(JAVAH) -o $@ $$classes

zbar.jar: $(zbar_jar_SRCS)
	cd $(abs_srcdir); $(JAVAC) -d $(abs_builddir) $(zbar_jar_SRCS)
	$(JAR) cf $@ $(zbar_jar_CLASSES) || $(RM) $@

check-local:
	echo "making check in java"
	cd $(abs_srcdir); $(JAVAC) -classpath $(CLASSPATH):$(abs_builddir)/zbar.jar -d $(abs_builddir) $(test_SRCS)
	$(top_builddir)/libtool -dlopen libzbarjni.la --mode=execute $(JAVA) -classpath zbar.jar:$(CLASSPATH):. -Xcheck:jni org.junit.runner.JUnitCore $(test_CLASSES)