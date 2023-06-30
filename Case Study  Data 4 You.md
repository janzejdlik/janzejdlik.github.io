## Case Study | Data 4 You

Case Study Documentation

```
Hi Jan,

As the next step of our selection process, we would like you to think through a case study and afterwards have a video interview with us. The objective behind the case is to evaluate your way of thinking and approaching problems, as well as your technical skills. We ask you to deliver the result using either JavaScript, PHP, or Java.  Deadline - please submit the case study at the latest by 9am on 26 June.

FIRST STEP. Case Study: Automated screenshot generator

Your task for the case study is to create an automated tool for taking screenshots of homepages of five selected webpages. There is a tool https://www.screenshotmachine.com/,which automatically creates these screenshots and even has an API. Please get familiar with this tool and the API and create an application that the API automatically saves these screenshots to Google Drive and then you share the Google Drive link with us. After you finish the case study upload your code to GitHub (or similar) and share it with us. Please make the code publicly accessible.

Files Format:

Use screenshot width: 1920 pixels and height of 1080 pixels, Image format JPG. Use the following structure of creating the name of the screenshot file “ID_name.jpg.

For the purpose of this case study please analyse these web pages:

id name  website
1 iFunded https://ifunded.de/en/
2 Property Partner www.propertypartner.co
3 Property Moose propertymoose.co.uk
4 Homegrown www.homegrown.co.uk
5 Realty Mogul https://www.realtymogul.com

SECOND STEP. Online Interview: During your online interview, we will ask you to guide us through your case study result.


Have a lovely day,


```

I have written the following Java Script code named picauto2.js in order to solve the case study mentioned above. 

```js
const screenshotmachine = require('screenshotmachine');
const fs = require('fs');
const { google } = require('googleapis');

const customerKey = '8d704a';
const screenshotWidth = 1920;
const screenshotHeight = 1080;
const screenshotFormat = 'jpg';
const folderId = '1pKUAsHUYi7FNiXdX6pNPk54EPx9IIK7v';

const websites = [
  { id: 1, name: 'iFunded', url: 'https://ifunded.de/en/' },
  { id: 2, name: 'Property Partner', url: 'https://www.propertypartner.co' },
  { id: 3, name: 'Property Moose', url: 'https://propertymoose.co.uk' },
  { id: 4, name: 'Homegrown', url: 'https://www.homegrown.co.uk' },
  { id: 5, name: 'Realty Mogul', url: 'https://www.realtymogul.com' }
];


// Authenticate Google Drive API
const auth = new google.auth.GoogleAuth({
  keyFile: '/home/nk/Desktop/Code/picauto/client_secret_653412893567-0ogqdrjrosfbbf1462dsceemfj43uaeu.apps.googleusercontent.com.json', // Path to your Google Cloud Platform service account credentials
  scopes: 'https://www.googleapis.com/auth/drive'
});

// Generate screenshots and save to Google Drive
async function generateAndSaveScreenshots() {
  const drive = google.drive({ version: 'v3', auth }); // Google Drive API Initialization

  // Loop iteration through websites and screenshot options
  for (const website of websites) {
    const options = {
      url: website.url,
      dimension: `${screenshotWidth}x${screenshotHeight}`,
      format: screenshotFormat,
      cacheLimit: '0',
      delay: '200',
      zoom: '100'
    };

    const apiUrl = screenshotmachine.generateScreenshotApiUrl(customerKey, '', options); // Generate Screenshot API URL
    const output = `screenshots/${website.id}_${website.name}.${screenshotFormat}`; // Where to save screenshot

    const response = await screenshotmachine.readScreenshot(apiUrl);
    response.pipe(fs.createWriteStream(output));

    await uploadToGoogleDrive(drive, output, website);
    console.log(`Screenshot saved and uploaded for ${website.name}`);
  }
}

// Upload screenshot file to Google Drive
async function uploadToGoogleDrive(drive, filename, website) {
  const fileMetadata = {
    name: `${website.id}_${website.name}.${screenshotFormat}`,
    parents: [folderId]
  };

  const media = {
    mimeType: `image/${screenshotFormat}`,
    body: fs.createReadStream(filename)
  };

  await drive.files.create({
    resource: fileMetadata,
    media: media,
    fields: 'id, webViewLink'
  });
}

generateAndSaveScreenshots();
```

As a first step towards increasing the chances of making the code functional I have changed the constant "customerKey" to the value of the Key given form "https://www.screenshotmachine.com/" while creating the free account. In my example it would be "8d704a"

```js
const customerKey = '8d704a';
const screenshotWidth = 1920;
const screenshotHeight = 1080;
const screenshotFormat = 'jpg';
const folderId = '1pKUAsHUYi7FNiXdX6pNPk54EPx9IIK7v';
```

Secondly step would be changing the Google Drive ID assigned under constant "folderId" to the value of "1pKUAsHUYi7FNiXdX6pNPk54EPx9IIK7v'

```js
const customerKey = '8d704a';
const screenshotWidth = 1920;
const screenshotHeight = 1080;
const screenshotFormat = 'jpg';
const folderId = '1pKUAsHUYi7FNiXdX6pNPk54EPx9IIK7v';
```

Thirdly I have assigned the path to the .json file, which I downloaded from Google Cloud, more specifically Google Drive API. I have also moved the .json file to the same directory as the picauto2.js, in my example "/home/nk/Desktop/Code/picauto/client_secret_653412893567-0ogqdrjrosfbbf1462dsceemfj43uaeu.apps.googleusercontent.com.json"

```js
// Authenticate Google Drive API
const auth = new google.auth.GoogleAuth({
  keyFile: '/home/nk/Desktop/Code/picauto/client_secret_653412893567-0ogqdrjrosfbbf1462dsceemfj43uaeu.apps.googleusercontent.com.json', // Path to your Google Cloud Platform service account credentials
  scopes: 'https://www.googleapis.com/auth/drive'
});
```

Furthermore, I have tried to check all of the three factors in order to check if they are correct. I have also made sure to download the following packages with npm:

```js
npm install screenshotmachine googleapis fs 
```

When trying to run the picauto2.js with node, I get the following error:

```js
node picauto2.js                            
/home/nk/Desktop/Code/picauto/node_modules/jwa/index.js:115
  return new TypeError(errMsg);
         ^

TypeError: key must be a string, a buffer or an object
    at typeError (/home/nk/Desktop/Code/picauto/node_modules/jwa/index.js:115:10)
    at checkIsPrivateKey (/home/nk/Desktop/Code/picauto/node_modules/jwa/index.js:61:9)
    at Object.sign (/home/nk/Desktop/Code/picauto/node_modules/jwa/index.js:147:5)
    at Object.jwsSign [as sign] (/home/nk/Desktop/Code/picauto/node_modules/jws/lib/sign-stream.js:32:24)
    at JWTAccess.getRequestHeaders (/home/nk/Desktop/Code/picauto/node_modules/google-auth-library/build/src/auth/jwtaccess.js:123:31)
    at JWT.getRequestMetadataAsync (/home/nk/Desktop/Code/picauto/node_modules/google-auth-library/build/src/auth/jwtclient.js:86:51)
    at JWT.requestAsync (/home/nk/Desktop/Code/picauto/node_modules/google-auth-library/build/src/auth/oauth2client.js:371:34)
    at JWT.request (/home/nk/Desktop/Code/picauto/node_modules/google-auth-library/build/src/auth/oauth2client.js:365:25)
    at GoogleAuth.request (/home/nk/Desktop/Code/picauto/node_modules/google-auth-library/build/src/auth/googleauth.js:674:23)
    at process.processTicksAndRejections (node:internal/process/task_queues:95:5)

Node.js v18.13.0
```

To this point I was not able to find a solution to this problem, any help form the outside would be highly appreciated.

Thank you