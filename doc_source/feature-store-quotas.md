# Quotas, Naming Rules and Data Types<a name="feature-store-quotas"></a>

## Limits and Quotas<a name="feature-store-limits-quotas"></a>
**Note**  
Soft limits can be increased based on your needs\.
+  **Maximum number of feature groups per AWS account:** Soft limit of 100\.
+  **Maximum number of feature definitions per feature group:** 2500\.
+  **Maximum Transactions per second \(TPS\) per API per AWS account:** Soft limit of 10000 TPS per API excluding the `BatchGetRecord` API call, which has a soft limit of 500 TPS\.
+  **Maximum size of a record:** 350KB\.
+  **Maximum size of a record identifier:** 2KB\. 
+  **Maximum size of a feature value:** 350KB\.
+ **Maximum number of concurrent feature group creation workflows:** 4\.
+ **BatchGetRecord API:** Can contain as many as 100 records and can query up to 10 feature groups\. 

## Naming Rules<a name="feature-store-naming-rules"></a>
+  **Reserved Words:** The following are reserved words and cannot be used as feature names in feature definitions: `is_deleted`, `write_time`, and `api_invocation_time`\. 

## Data Types<a name="feature-store-data-types"></a>
+  **String Feature Type:** Strings are Unicode with UTF\-8 binary encoding\. The minimum length of a string can be zero, the maximum length is constrained by the maximum size of a record\. 
+  **Fractional Feature Type:** Fractional feature values must conform to a double precision floating point number as defined by the [IEEE 754 standard](https://en.wikipedia.org/wiki/IEEE_754)\. 
+  **Integral Feature Type:** Feature Store supports integral values in the range of a 64\-bit signed integer\. Minimum value of \-263 and a maximum value: 263 \- 1\. 
+  **Event Time Features:** All feature groups have an event time feature\. The feature can have a feature type of either String or Fractional\. A string event time is accepted in ISO\-8601 format, in UTC time, conforming to the pattern\(s\): \[yyyy\-MM\-dd'T'HH:mm:ssZ, yyyy\-MM\-dd'T'HH:mm:ss\.SSSZ\]\. A fractional event time value is accepted as seconds from unix epoch, with millisecond precision\. Event times must be in the range of \[0000\-01\-01T00:00:00\.000Z, 9999\-12\-31T23:59:59\.999Z\]\. 