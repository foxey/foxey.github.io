CONVERT = /usr/bin/convert

SRC_COVER := $(wildcard *.cover)
SRC_750 := $(wildcard *.750)
SRC_THUMBNAIL := $(wildcard *.thumbnail)

OBJ_COVER := $(SRC_COVER:.cover=-cover.jpg)
OBJ_750 := $(SRC_750:.750=-750.jpg)
OBJ_THUMBNAIL := $(SRC_THUMBNAIL:.thumbnail=-thumbnail.jpg)

SRC :=  $(SRC_COVER) $(SRC_750) $(SRC_THUMBNAIL)
OBJ :=  $(OBJ_COVER) $(OBJ_750) $(OBJ_THUMBNAIL)

JPG := $(shell ls *.jpg|grep -v -e "\-cover.jpg$$" -e "\-thumbnail.jpg$$" -e "\-750.jpg$$")

all : ${OBJ}

clean :
	${RM} *-cover.jpg *-750.jpg *-thumbnail.jpg

clobber : clean
	$(RM) *.cover *.750 *.thumbnail

convert : convert.sh
	tail -n +2 convert.sh|sed -e 's/^[^ ]* [^ ]* \(.*\) \([^ ]*\)-\([^ ]*\).jpg$$/echo \1 > \2.\3.tmp/' | sh -
	for file in $$(ls *.tmp); do mv -n $$file $$(basename $$file .tmp);done
	${RM} *.tmp

targets : $(JPG:.jpg=.750) $(JPG:.jpg=.thumbnail)

%-cover.jpg : %.cover %.jpg
	${CONVERT} $(basename $<).jpg $(shell cat $<) $@

%-750.jpg : %.750 %.jpg
	${CONVERT} $(basename $<).jpg $(shell cat $<) $@

%-thumbnail.jpg : %.thumbnail %.jpg
	${CONVERT} $(basename $<).jpg $(shell cat $<) $@

%.thumbnail : | %.jpg
	echo '-resize 750' > $@

%.750 : | %.jpg
	echo '-crop 750x750+0000+0500 -resize 750x750' > $@

%.cover : | %.jpg
	echo '-crop 9999x9999+0000+0000' > $@
