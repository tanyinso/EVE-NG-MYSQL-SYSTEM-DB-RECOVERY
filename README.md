# EVE-NG MySQL System Database Recovery

This repository documents a real-world issue encountered on an EVE-NG server where the MySQL service failed to start due to a missing or corrupted system database.

## Problem Description

After disk space exhaustion and cleanup on an Ubuntu-based EVE-NG server, MySQL failed to start and returned errors such as:

- MySQL service not running
- MySQL system database not found
- Missing files under `/var/lib/mysql`
- EVE-NG web UI inaccessible

This issue can completely break an EVE-NG lab environment if not handled correctly.

## Root Cause

The MySQL system database became corrupted or partially deleted while the disk was full, leading to missing internal MySQL tables required for startup.

## What This Repository Covers

- Verifying MySQL service and error logs
- Identifying corrupted or missing MySQL system databases
- Safely rebuilding MySQL system tables
- Restoring MySQL service functionality
- Validating EVE-NG web UI after recovery

## Environment

- EVE-NG (Community/Pro)
- Ubuntu Server
- MySQL Server
- LVM-backed storage
