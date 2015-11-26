# Adding Images to Wiki Pages #

The following outlines the procedure to add images to wiki pages [Reference: Comment by Woody](http://code.google.com/p/support/wiki/WikiSyntax)

```
svn checkout https://mini2440-friendlyarm.googlecode.com/svn/wiki/ mini2440-friendlyarm --username your-username

cd mini2440-friendlyarm

# suggested good practice is to create a directory
# for attachments for each wiki page
mkdir WikiPageName.attach

cp ~/image_name.png WikiPageName.attach/

svn add WikiPageName.attach
```

Edit your wiki page to add the image links

```
vi WikiPageName.wiki

# Add the link to your image    
http://mini2440-friendlyarm.googlecode.com/svn/wiki/WikiPageName.attach/imagename.png

svn commit -m "Adding images and links for WikiPageName" WikiPageName
```

Go check your wiki page out, the updated page with the images should be there.

Any new images can be added just as easily.

```
svn add WikiPageName.attach/newimage.png
vi WikiPageName.wiki
svn commit -m "Adding newimage.png to WikiPageName" WikiPageName.attach/newimage.png 
```