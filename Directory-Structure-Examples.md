When integrating Intern with an existing project, or when starting a new project, it’s useful to understand which directory structures are suitable for use with Intern and which structures will need to be modified to conform to the way Intern works. The only structural restriction is that the entire project, including all dependencies, need to be accessible from the directory located two levels above the Intern directory.

The following are valid directory structures for use with Intern:

1. Entire application in root directory, Intern installed in root

   ```
   /
     app/
     depA/
     node_modules/
       intern/
   ```

2. Application in subdirectory of root directory, Intern installed in root

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

3. Intern installed globally, referenced as symlink

   ```
   /
     app/
     depA/
     node_modules/
       intern/ -> /usr/local/lib/nodejs/node_modules/intern
   ```

The following would *not* be a valid directory structure:

1. Intern installed inside application directory, dependencies outside application directory

   ```
   /
     app/
       node_modules/
         intern/
     depA/
   ```

2. Intern installed in subdirectory that is a sibling of the project

   ```
   /
     project/
       app/
       depA/
     support/
       node_modules/
         intern/
   ```