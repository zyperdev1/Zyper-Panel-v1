# Zyper-Panel-v1
There are Some Bugs in it so we are currently fixing it and you can report the bugs in the ZyperDevelopment server

Copy the commands to install Zyper-Panel-v1

cloning the files
```
git clone https://github.com/zyperdev1/Zyper-Panel-v1
```
Going to the Directory
```
cd ZyperPanel-v1
```
installing Node.js/npm deficencies
```
npm install
```
renaming example.env to .env
```
cp example.env .env
```
Bulding the panel
```
npm run migrate:dev && npm run build && npm run seed
```

start the panel

```
npm run start
```
