## Multipart + Presigned URL upload to AWS S3/Minio via the browser

### Motivation

I created this demo repo because documentation for multipart uploading of large files using presigned URLs was very scant.

I wanted to create a solution to allow users to upload files directly from the browser to AWS S3 (or any S3-compliant storage server). This worked great when I used AWS SDK's getSignedUrl API to generate a temporary URL that the browser could upload the file to. 

However, I hit a snag when dealing with files > 5GB because the pre-signed URL only allows for a maximum file size of 5GB to be uploaded at one go. As such, this repo demonstrates the use of multipart + presigned URLs to upload large files to an AWS S3-compliant storage service.

### Components used in this demo

Frontend Server: React (Next.js)
Backend Server: Node.js (Express), using the AWS JS SDK
Storage Server: Minio (but this can easily be switched out to AWS S3)

### How to run

* Clone the repo and change directory into the repo
* Open three different terminal windows.

**Storage Server**

In window 1, run:
```
# Set up the Minio server (ignore this if you are using AWS S3)
# Minio docs: https://docs.minio.io/docs/minio-quickstart-guide
minio server /data
```
> Note: Set the S3-compliant bucket policy as appropriate to allow the right access

**Backend Server**

Replace the following code in `backend/server.js` with your AWS S3 or S3-compliant storage server config.

```
const s3  = new AWS.S3({
  accessKeyId: '<ACCESS_KEY_ID>' , // Replace with your access key id
  secretAccessKey: '<SECRET_ACCESS_KEY>' , // Replace with your secret access key
  endpoint: 'http://127.0.0.1:9000' ,
  s3ForcePathStyle: true, // needed with minio?
  signatureVersion: 'v4'
});
```

Note: If you are using AWS S3, follow the docs on the AWS website to instantiate a new AWS S3 client.

In window 2, run:
```
cd backend
npm install
node server.js
```

**Frontend Server**

In window 3, run:
```
cd frontend
npm install
npm run dev
```

**Upload File**

Go to `http://localhost:3000` in your browser window and upload a file.