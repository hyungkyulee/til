# build issues on Node Packages

### fsevents
#### issue
```
```

####
https://github.com/nodejs/node-gyp/issues/1547 

```
rm -rf package-lock.json
rm -rf node_modules
npm cache clean --force
npm install -g --unsafe-perm node-gyp
( or npm install -g --unsafe-perm=true --allow-root)

npm i -f
```
