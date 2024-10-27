# Deploy react app to S3 Bucket with github actions

![Screenshot 2024-10-27 at 2 03 09 PM](https://github.com/user-attachments/assets/d0357704-29de-4838-8537-b41d97795311)

## Prerequisites:
1. Create React app and add it to repo.
2. Create Iam user in AWS and get Key and secret and add it to Github secrets variables in Settings of the project.
3. Create a S3 Bucket with all permissions
   
## Steps:
1. Create a Workflow file going to Github action.
2. Add this code in main.yml file, this does all ths steps described in above image.

```
name: Deploy to AWS S3

on:
  push:
    branches:
      - main
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout,
        uses: actions/checkout@v1

      - name: NodeJs Setup
        uses: actions/setup-node@v1
        with:
          node-version: "18"
      
      - name: Dependency Installation
        run: npm install

      - name: App Build
        run: npm run build

      - name: Configure AWS Credentials
        uses: aws-actions/configure-aws-credentials@v1
        with:
          aws-access-key-id: ${{secrets.aws_key}}
          aws-secret-access-key: ${{secrets.aws_secret}}
          aws-region: ${{secrets.region}}

      - name: Deploy to AWS S3
        run: aws s3 sync build ${{secrets.s3bucket_name}} --delete
```

3. Go to properties in S3 and scroll to bottom and see enable static hosting, enable it and add index.html as intial page. Then you will get the url.
