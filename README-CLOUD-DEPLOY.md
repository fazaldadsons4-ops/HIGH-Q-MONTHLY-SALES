# HIGH Q Pharma Suite — Cloud deployment

Upload HIGH-Q-PHARMA-CLOUD-DEPLOY.zip to the root of this branch. Render Blueprint reads render.yaml and extracts the ZIP during build.

The app has three sections: PDF Cleaner & Column Mapper, Monthly Sales Calculator, and Pharma Sales Consolidation.

No database is used. Uploaded files and generated results are processed in memory/session state. Public means anyone with the URL can use the app; do not upload confidential customer, patient, or personal data.

Render Free services sleep after 15 minutes of inactivity and use ephemeral storage. First wake-up can take about a minute. Validate all outputs against known source files before using for official monthly closing.
