## Create "thumbs"
```bash
#!/bin/bash
for filename in $(find -name '*.jpg'); do
	FNAME=$(basename $filename);
    if [[ ! $FNAME == thumb* ]]
	then
		echo "$(dirname $filename)/thumb-$(basename $filename)";
	    convert -thumbnail 150x150 $filename "$(dirname $filename)/thumb-$(basename $filename)" ;
	fi
done
```

## Make watermarks
```bash
#!/bin/bash
#make watermark
for filename in $(find -name '*.jpg'); do
	FNAME=$(basename $filename);
    if [[ ! $FNAME == thumb* ]]
	then
		#echo $filename;
	    composite -gravity SouthWest watermark.png $filename $filename
	fi
	jpegoptim --strip-all --max=85 $filename
done
```

## Optimize JPG
```bash
#!/bin/bash
dir=$1
if [ -n "$dir" ]
then
   cd $dir
   for filename in $(find -name '*.jpg'); do
      jpegoptim --strip-all --max=85 $filename
   done
fi
```

### Crop {2} pixels in {1} folder from top and bottom
```bash
#!/bin/bash
dir=$1
height=${2:-28}
mogrify -chop 0x$height+0+0 -gravity South ./$dir/*.jpg
mogrify -chop 0x$height+0+0 -gravity North ./$dir/*.jpg
```

## PDF to JPG-files
```bash
#!/bin/bash
dir=$1
fname=$2
mkdir $1
pdftoppm $fname.pdf ./$dir/output -jpeg
```

## Incremental renaming
```bash
#!/bin/bash
dir=$1
i=${2-1}
if [ -n "$dir" ]
then
   cd $dir
   for filename in `ls *.jpg | sort -V`; do
      mv $filename $i.jpg
      let i++;
   done
fi
```

## Resize images
```bash
#!/bin/bash
dir=$1
if [ -n "$dir" ]
then
   cd $dir
   for filename in $(find -name '*.jpg'); do
      convert $filename -resize 700x $filename
   done
fi
```

## Optimize images of the last day
```bash
#!/bin/bash
find ~/some_dir_with_images -mtime -1 -iname *.jpg -exec jpegoptim --strip-all --max=85 -p {} \;
find ~/some_dir_with_images -mtime -1 -iname *.png -exec optipng -o7 -preserve {} \;
```