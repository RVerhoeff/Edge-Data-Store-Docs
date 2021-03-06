---
uid: SystemConfiguration
---

# System configuration

Edge Data Store uses JSON configuration files in a protected directory on Windows and Linux to store configuration that is read on startup. While the files are accessible to view, OSIsoft recommends that you use REST or the EdgeCmd command line tool to make any changes to the files. 

As part of making Edge Data Store as secure as possible, any passwords or secrets that you configure are stored in encrypted form (with cryptographic key material stored separately in a secure location.) If you edit the files directly, the system may not work as expected.

**Note:** You can edit any single component or facet of the system using REST, but also configure the system as a whole with a single REST call.
