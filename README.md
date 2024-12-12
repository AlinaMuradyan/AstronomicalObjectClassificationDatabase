Celestial Object Database Management

This repository contains the SQL schema and Python script for creating, managing, and querying a celestial object database. The project integrates with the Gaia database using ADQL queries and stores results in a PostgreSQL database hosted on ElephantSQL. The schema is designed to handle diverse astronomical data, including object types, celestial objects, criteria, and historical changes.

Features

Comprehensive database schema for celestial objects, their types, criteria, and historical records.

Integration with Gaia TAP service to fetch and process astronomical data using ADQL.

PostgreSQL database management with SQLAlchemy.

Example queries and data mappings for both numeric and categorical criteria.

Prerequisites

Python 3.7+

PostgreSQL database (or ElephantSQL account)

Required Python packages:

pyvo

astroquery

sqlalchemy

pandas

psycopg2-binary

Setup Instructions

Database Setup

Create a PostgreSQL database. If using ElephantSQL, follow their setup instructions to obtain connection credentials.

Run the provided SQL script (schema.sql) to create the database tables.

psql -h <host> -U <username> -d <database> -f schema.sql

Python Environment Setup

Install the required Python packages:

pip install pyvo astroquery sqlalchemy pandas psycopg2-binary

Update the connection_string in the Python script to match your PostgreSQL database credentials:

connection_string = f"postgresql+psycopg2://<username>:<password>@<host>:<port>/<database>"

Run the Python script to populate the database and execute queries:

python script.py

Schema Overview

Tables

object_types: Defines celestial object types (e.g., stars, galaxies).

celestial_object: Stores celestial objects and their coordinates.

criteria: Defines criteria for celestial objects (e.g., magnitude, mass).

criteria_category: Stores categorical criteria values (e.g., spectral type).

celestial_object_criteria_numeric: Stores numeric criteria values for celestial objects.

celestial_object_criteria_category: Stores categorical criteria values for celestial objects.

history: Tracks changes to celestial objects.

stars_spectral_type_temperature: Maps spectral types to temperature ranges.

constellations: Stores constellation data.

stars_data: Contains additional data specific to stars.

Sample Data

object_types: Contains entries like "Star", "Galaxy", "Quasar".

criteria: Includes photometric magnitude with units (e.g., "mag").

Gaia source data is mapped to celestial_object with example star objects.

Example Queries

Fetch Data

Top 10 celestial objects:

SELECT * FROM celestial_object LIMIT 10;

Star criteria:

SELECT co.object_name, c.criteria_name, cocn.value
FROM celestial_object AS co
JOIN celestial_object_criteria_numeric AS cocn ON co.object_id = cocn.object_id
JOIN criteria AS c ON cocn.criteria_id = c.criteria_id
WHERE co.object_type_id = 1
ORDER BY co.object_name;

Insert Example Data

The Python script includes functionality to:

Insert object_types, celestial_object, criteria, and numeric criteria data.

Example mapping Gaia data to schema:

celestial_data = pd.DataFrame({
    "object_type_id": [1] * len(df),
    "object_name": ["Gaia-" + str(sid) for sid in df["SOURCE_ID"]],
    "right_ascension": df["ra"],
    "declination": df["dec"]
})

Project Goals

This project provides a robust framework for managing astronomical data. By integrating ADQL with PostgreSQL, it enables efficient querying and detailed analysis of celestial objects.

Contributing

Contributions are welcome! Please fork the repository and submit a pull request with your changes.

License

This project is licensed under the MIT License. See the LICENSE file for details.
