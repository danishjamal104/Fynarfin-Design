# FaQs regarding importer-rdbms in bulk-disbursement use-case

**Q1. How batch summary API works?** <br>
Ans. Batch summary API aggregates data from `transfers` table and add the count in `batches` table.

**Q2. How batch details API works?** <br>
Ans. Batch details API queries the `transfers` table and returns the detail in a paginated manner.

**Q3. Why importer-rdbms is accessing AWS S3 bucket?** <br>
Ans. For bulk-connectors scenario there is no way we are inserting data into the `transfers` table directly. This will make `batch-summary` and `batch-details` not return the expected result. So we are using CSV file stored in the AWS S3 bucket, importer-rdbms will read the CSV file from S3 bucket and insert the data into the transfers table.

**Q4. Is importer-rdbms responsible for updating data/status of transaction in CSV file?** <br>
Ans. No, the CSV is updated by the respective bulk-connectors.

**Q5. How importer-rdbms is updating the status of transaction in `transfers` table?** <br>
Ans. By reading data from CSV file stored in AWS S3 bucket and then inserting/updating data manually. Refer Q3.

**Q6. What is the trigger for importer-rdbms to read the CSV file from AWS S3 bucket?** <br>
Ans. When we receive `ELEMENT_COMPLETED` event for `bulk-connector` workflow for the `PROCESS_INSTANCE` event of zeebe workflow.

**Q7. What regression would be caused if we remove S3 access in importer-rdbms?** <br>
Ans. We will not be able to insert data into `transfers` table and `batch-summary` and `batch-details` API will not return the expected result in the case of bulk-connectors.

# Miscellaneous FaQs related to bulk-processor

**Q1. Why we are not using kafka in bulk-processor for streaming records?** <br>
Ans. First reason is, since data set can be large, so we won't be able to implement staging phase since records are published on one by one basis. Secondly service which are consuming the kafka topic will have to add kafka configuration.

**PS: Please refer [this document](https://docs.google.com/document/d/1DJuRTwmFyvFWw9-90Mx58iLDXOJfeVdfnycf8xtywbE/edit#heading=h.stun1h14j592) for additional FaQs regarding bulk-processor and bulk disbursement use-case.**
