MANGO_ROOT = ../../
PREBUILT_DOC = /z/doc/build/html

all: installer

staging:
	mkdir -p staging

installer: staging staging/include staging/script staging/lib staging/doc staging/sample \
     staging/core staging/mango-a-493-312.bmp 
	makensis -DPRODUCT_VERSION=`git describe` mango-setup.nsi

staging/include: 
	cp -R $(MANGO_ROOT)/include staging/

staging/core: 
	mkdir -p staging/core
	cp $(MANGO_ROOT)/build/src/mango.exe staging/core/
	cp $(MANGO_ROOT)/dlls/freeglut.dll staging/core/
	cp $(MANGO_ROOT)/dlls/libstdc++-6.dll staging/core/
	cp $(MANGO_ROOT)/dlls/libgcc_s_dw2-1.dll staging/core/
	cp $(MANGO_ROOT)/dlls/python32.dll staging/core/
	cp -R $(MANGO_ROOT)/python-redist/* staging/core/

staging/lib:
	mkdir -p staging/lib
	cp $(MANGO_ROOT)/build/src/libmango_core.a staging/lib/
	cp $(MANGO_ROOT)/build/src/libmango_geometry.a staging/lib/
	cp $(MANGO_ROOT)/build/src/libmango_onglut.a staging/lib/
	cp $(MANGO_ROOT)/build/src/libmango_py.a staging/lib/

staging/script:
	mkdir -p staging/script
	cp $(MANGO_ROOT)/build/src/Geometry.pyd staging/script/Geometry.pyd
	cp $(MANGO_ROOT)/samples_mangopy/mangopy/demo.py staging/script/

staging/sample:
	mkdir -p staging/sample
	cp $(MANGO_ROOT)/samples_mangopy/mangopy/*.py staging/sample/

staging/doc:
	cp -R $(PREBUILT_DOC) staging/doc
#	cp -R $(MANGO_ROOT)/doc/build/html staging/doc

staging/mango-a-493-312.bmp:
	cp mango-a-493-312.bmp staging/

clean:
	rm -rf staging
	rm -f Mango*.exe
