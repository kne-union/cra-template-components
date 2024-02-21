由于cra限制，模板不能拿到project-name，初始化完成后需要再手动进行一些修改，后期会替换掉cra

1. package.json

   删除
    ```json
        {"private":true}
    ```

   修改
    ```json
    {"name": "project-name"}
    ```
   为
    ```json
    {"name": "@kne/project-name"}
    ```
   添加
    ```json
    {"files": ["build"]}
    ```
2. 

   将components-name替换为实际项目名
    ```json
    {
      "scripts": {
        "build": "cross-env COMPONENTS_NAME=components-name MODULES_DEV_PUBLIC_URL=/components-name  craco build"
      }
    }
    ```

2. src/preset.js
   替换其中的components-name为实际项目名
   ```js
       remoteLoaderPreset({
           remotes: {
               default: componentsCoreRemote,
               'components-core': componentsCoreRemote,
               'components-name': process.env.NODE_ENV === 'development' ? {
                   remote: 'components-name', url: '/', tpl: '{{url}}'
               } : {
                   remote: 'components-name',
                   url: 'https://registry.npmmirror.com',
                   tpl: '{{url}}/@kne%2f{{remote}}/{{version}}/files/build',
                   defaultVersion: process.env.DEFAULT_VERSION
               }
           }
       });
    ```
3. .github/workflows/publish.yml

   将packageName替换为package.json中的name
    ```yml
    - name: Sync To Cnpm
      run: npm i -g cnpm && cnpm sync packageName
    ```

