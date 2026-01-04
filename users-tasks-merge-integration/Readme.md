# Merging Users with Their Tasks Integration

## Description
This Boomi integration retrieves users and their related tasks from separate
REST APIs, processes and splits the data, and then merges each user with
their corresponding tasks into a unified document structure.

## Integration Flow
- Starts the process without initial data
- Calls a Tasks REST API using HTTP Client connector
- Splits task documents by task identifier
- Temporarily stores task data using Document Cache
- Calls a Users REST API using HTTP Client connector
- Splits user documents by user identifier
- Merges users with their related tasks using a Map shape
- Combines documents by user
- Ends the process after successful execution

## Key Concepts Used
- REST API Integration
- HTTP Client Connector
- Document Splitting
- Document Cache
- Data Correlation
- Data Mapping
- Document Combining

## Tools & Technologies
- Boomi AtomSphere
- HTTP Client Connector
- JSON Profiles
- Document Cache
- Map Shape

## Screenshots
Screenshots illustrate the process flow, API calls, document splitting,
data merging, and final output structure.

