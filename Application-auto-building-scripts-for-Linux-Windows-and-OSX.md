build.sh for Linux:
```
#zip all files to nw archive
zip -r my-app.nw ./*
#copy nw.pak from current build node-webkit
cp /opt/node-webkit/nw.pak ./nw.pak
#compilation to executable form
cat /opt/node-webkit/nw ./my-app.nw > ../build/linux/my-app && chmod +x ../build/linux/my-app
#move nw.pak to build folder
mv ./nw.pak ../build/linux/nw.pak
#remove office-improver.nw
rm ./office-improver.nw
#run application
../build/linux/my-app
```