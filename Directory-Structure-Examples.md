*This information is obsolete as of Intern 1.3. You may set `loader.baseUrl` to any value in Intern 1.3, which enables you to use arbitrary directory structures.*

When integrating Intern with an existing project, or when starting a new project, it’s useful to understand which directory structures are suitable for use with Intern and which structures will need to be modified to conform to the way Intern works. The only structural restriction is that the entire project, including all dependencies, need to be accessible from the directory located two levels above the Intern directory.

## Valid structures

1. Entire application in root directory, Intern installed in root

   ```
   /
     app/
     depA/
     node_modules/
       intern/
   ```

2. Application & dependencies in subdirectory of root directory, Intern installed in root

   ```
   /
     src/
       app/
       depA/
     node_modules/
       intern/
   ```

   (or)

   ```
   /
     project/
       src/
         app/
         depA/
     node_modules/
       intern/
   ```

3. Application in root, dependencies in a subdirectory, Intern installed in root

   ```
   /
     app/
     bower_components/
       depA/
     node_modules/
       intern/
   ```

4. Intern installed globally, referenced as symlink from the root

   ```
   /
     app/
     depA/
     node_modules/
       intern/ -> /usr/local/lib/nodejs/node_modules/intern
   ```

## Invalid structures

1. Intern installed inside application directory, dependencies siblings of the application directory

   ```
   /
     app/
       node_modules/
         intern/
     depA/
   ```

2. Application in root, Intern installed inside a subdirectory

   ```
   /
     app/
     depA/
     support/
       node_modules/
         intern/
   ```