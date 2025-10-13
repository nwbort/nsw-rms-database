# NSW live traffic data scraper

This repository contains a scheduled scraper that downloads live traffic hazard data from Transport for NSW. The data is updated daily and includes information on road incidents, roadworks, fires, floods, and other major events impacting the road network in New South Wales, Australia.

## Data source

The data is sourced from the official Live Traffic NSW website, which provides GeoJSON feeds for various types of traffic hazards. This data is made available by Transport for an NSW under a Creative Commons Attribution License.

The scraper downloads data from the following endpoints:
- incidents
- roadworks
- floods
- alpine conditions
- fires
- major events

## Repository structure

The repository is organized as follows:

- **`download.sh`**: A script that downloads a file from a given URL and saves it with a filename derived from that URL. It also automatically detects the MIME type and adds the correct file extension. For JSON files, it will try to pretty-print the content for better readability.
- **`scrape.sh`**: This script executes the `download.sh` script for each of the traffic hazard data endpoints.
- **`.github/workflows/scrape.yml`**: A GitHub Actions workflow that automates the scraping process. It runs on a daily schedule, executes the `scrape.sh` script, and commits the updated data files to the repository.
- **`*.json`**: The scraped data files in JSON format. Note that some files may be empty if there are no active hazards of that type.
- **`*.xml`**: The scraped data files in XML format.

## How it Works

The scraping process is fully automated using GitHub Actions. The `scrape.yml` workflow is configured to run 23 minutes past the hour.

The workflow performs the following steps:
1. checks out the repository.
2. runs the `scrape.sh` script to download the latest traffic data.
3. commits the updated files to the repository with a timestamp.

This ensures that the repository always contains the most recent traffic hazard data.

## Usage

You can use this repository in several ways:
- **access the data**: The downloaded JSON files are directly available in the repository for analysis or use in your own applications.
- **fork the repository**: You can fork this repository to modify the scraper, add new data sources, or integrate the data into your own projects.

## Data format

The data is provided in GeoJSON format. Each file contains a `FeatureCollection` object with an array of features representing individual traffic hazards.

Each feature includes a geometry (usually a point) and a set of properties that provide detailed information about the hazard, such as:
- the type of hazard (e.g., "BUILDING FIRE", "GRASS FIRE")
- the location (suburb, region, coordinates)
- the roads affected
- advice for motorists
- the last update time

For a detailed description of the data format, please refer to the official documentation from Transport for NSW.
