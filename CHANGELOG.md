# Change Log

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](http://keepachangelog.com/)
and this project adheres to [Semantic Versioning](http://semver.org/).

## 0.5.5

### Fixed
- use new repository for postgresql chart

## Changed
- upgraded postgresql to 14.5

## 0.5.4

### Fixed
- back to hooks since job completion requires RBAC role

## 0.5.3

### Fixed
- need to check for table before start bety application

## 0.5.2

### Added
- use new check image to use PG environment variables
- add-user and load-db are now jobs, not hooks (prevent timeout issues)

## 0.5.1

## Changed
- update README to describe values
- fix left over when initializing from URL
- fix binami url change

## 0.5.0

## Added
- initial release of the BETY helm chart.
- build on bety 5.4.1