# S3 Lifecycle Rules Configuration

This repository contains a fully functional **AWS S3 Lifecycle Rules** configuration designed to manage storage tiering, retention, and cleanup for your S3 buckets.  

The rules included in this configuration are optimized for cost efficiency and data lifecycle management.

---

## Lifecycle Rules Overview

### 1. Tier to IA → Glacier → Deep Archive, then Delete
- Moves objects to **STANDARD_IA** after 30 days.
- Moves objects to **GLACIER** after 90 days.
- Moves objects to **DEEP_ARCHIVE** after 365 days.
- Deletes objects after 730 days (2 years).
- Aborts incomplete multipart uploads after 7 days.

### 2. Tag-Based Cold Archive
- Applies only to objects tagged with `class=logs` and larger than 128 KB.
- Moves objects to **DEEP_ARCHIVE** after 180 days.
- Retains them for 7 years (2555 days).

### 3. Versioned Bucket Hygiene
- Applies to versioned objects with prefix `docs/`.
- Moves noncurrent versions to **STANDARD_IA** after 30 days, then to **GLACIER** after 90 days.
- Deletes noncurrent versions older than 365 days, keeping only the 10 most recent noncurrent versions.
- Removes expired object delete markers.

### 4. Immediate Intelligent-Tiering
- Moves all new objects to **INTELLIGENT_TIERING** immediately.
- Ideal for unpredictable access patterns.

---

## File Structure

- `lifecycle.json` — Contains the complete lifecycle configuration in JSON format.

---

## How to Apply Lifecycle Rules

### Using AWS CLI
1. Save the lifecycle configuration file locally (e.g., `lifecycle.json`).
2. Run the following command:

```bash
aws s3api put-bucket-lifecycle-configuration --bucket your-bucket-name --lifecycle-configuration file://lifecycle.json
