

#### setup nextjs
- npx create-next-app --typescript aiciti-web-v2
```
npx create-next-app --typescript aiciti-web-v2                             127 ✘ │ 16:48:42
Need to install the following packages:
  create-next-app@14.2.2
Ok to proceed? (y) y
✔ Would you like to use ESLint? … No / Yes
✔ Would you like to use Tailwind CSS? … No / Yes
✔ Would you like to use `src/` directory? … No / Yes
✔ Would you like to use App Router? (recommended) … No / Yes
✔ Would you like to customize the default import alias (@/*)? … No / Yes
Creating a new Next.js app in /Users/kyu/dev/ainsight/aiciti-web-v2.

Using npm.

Initializing project with template: app-tw


Installing dependencies:
- react
- react-dom
- next

Installing devDependencies:
- typescript
- @types/node
- @types/react
- @types/react-dom
- postcss
- tailwindcss
- eslint
- eslint-config-next


added 361 packages, and audited 362 packages in 32s

133 packages are looking for funding
  run `npm fund` for details

found 0 vulnerabilities
Initialized a git repository.

Success! Created aiciti-web-v2 at /Users/kyu/dev/ainsight/aiciti-web-v2
```
- build/run
```
npm i
npm run build
npm run start
```

#### setup cornerstone
- based on Cornerstone3D library
```
npm install @cornerstonejs/core
npm install @cornerstonejs/tools
npm install @cornerstonejs/streaming-image-volume-loader
npm install @cornerstonejs/calculate-suv
npm install @cornerstonejs/dicom-image-loader
```

#### three
```
npm install three @types/three
```








