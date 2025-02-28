# Café Static Website on Amazon S3

This repository contains the code and documentation for launching a static website for a café using Amazon S3. The website visually showcases the café’s offerings and provides essential business details such as location, business hours, and telephone number. This project was developed as part of an AWS lab exercise, demonstrating key S3 features including static website hosting, public access configuration, versioning, lifecycle management, and Cross-Region Replication (CRR) for disaster recovery.

---

## Architecture Overview

The solution uses two S3 buckets in different AWS regions:
- **Primary Bucket (US East - N. Virginia):** Hosts the static website files.
- **Secondary Bucket (Different Region):** Receives replicated objects from the primary bucket via CRR to ensure enhanced durability and disaster recovery.

You can view the architecture diagram below (located in the same repository as `s3_static_site.svg`):

![Architecture Diagram](s3_static_site.svg)

---

## Project Breakdown

### Challenge 1: Launching the Static Website

**Tasks:**
- **Extract Files:**  
  - Download and extract the provided ZIP file containing the website files (index.html, CSS files, and the images folder).
  
- **Create an S3 Bucket for Hosting:**  
  - Create an S3 bucket in the US East (N. Virginia) region.
  - Configure static website hosting using `index.html` as the index document.
  - Clear the "Block all public access" setting and enable ACLs.

- **Upload Content:**  
  - Upload `index.html`, CSS files, and the images folder to your S3 bucket.
  - Open the website endpoint to verify the site is live.

- **Set Public Read Access:**  
  - Create and attach a bucket policy to grant read-only access for anonymous users, ensuring that your website is publicly accessible.

---

### Challenge 2: Protecting Website Data

**Task:**
- **Enable Versioning:**  
  - Turn on versioning for the S3 bucket to prevent accidental overwrites or deletions.
  - Make minor modifications to `index.html` (for example, change embedded CSS colors).
  - Upload the updated file and verify multiple versions exist using the “Show versions” feature.

---

### Challenge 3: Optimizing Storage Costs with Lifecycle Policies

**Task:**
- **Set Lifecycle Policies:**  
  - Configure two separate lifecycle rules in the website bucket:
    - **Rule 1:** Transition previous versions of objects to S3 Standard-Infrequent Access (Standard-IA) after 30 days.
    - **Rule 2:** Permanently delete previous versions after 365 days.

These rules help manage storage costs by automatically moving or retiring older versions of the website files.

---

### Challenge 4: Enhancing Durability & Disaster Recovery with CRR

**Task:**
- **Enable Cross-Region Replication (CRR):**  
  - In a different AWS region, create a secondary S3 bucket and enable versioning on it.
  - On the primary S3 bucket, set up CRR to replicate the entire bucket to the secondary bucket.
  - Use the `CafeRole` IAM role (with permissions such as `s3:ReplicateObject`, `s3:ReplicateDelete`, and `s3:ReplicateTags`) to allow replication.
  - Validate that updates to the primary bucket are replicated to the secondary bucket and that deletions in the source bucket mirror in the destination bucket.

This replication strategy safeguards the website data by providing an additional backup in a separate region, supporting disaster recovery efforts.

---

## How to Use the Website

1. **Access the Website:**  
   - Use the endpoint URL provided by the S3 bucket or configure your custom domain to point to the bucket.

2. **Update Content:**  
   - Make changes locally to HTML, CSS, or images.
   - Upload the updated files to the primary S3 bucket.
   - Versioning, lifecycle rules, and CRR will help manage updates and maintain data integrity.

3. **Monitor Replication and Lifecycle Actions:**  
   - Check the secondary bucket to ensure CRR is functioning correctly.
   - Monitor older versions via the S3 console to see lifecycle transitions and deletions in action.

---

## Lessons Learned & Best Practices

- **Static Website Hosting:** Amazon S3 offers a scalable, cost-effective solution for hosting static sites.
- **Security:** A properly configured bucket policy ensures the website is publicly accessible while adhering to AWS security guidelines.
- **Data Protection:** Enabling versioning safeguards your website against accidental data loss.
- **Cost Management:** Lifecycle policies help optimize storage costs by automating data transitions and expirations.
- **Disaster Recovery:** Cross-Region Replication provides a robust backup mechanism, ensuring high availability and resilience against regional outages.

---

## Contributing

This project was developed as part of an AWS lab exercise. Contributions, feedback, and improvements are welcome! Please open issues or submit pull requests to help enhance this project.

---

## License

This project is licensed under the [MIT License](LICENSE).

