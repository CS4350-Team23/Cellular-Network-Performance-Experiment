# Troy NY cellular field readings and FCC modeled coverage

This dataset compares phone-observed cellular signal strength and connection performance around Rensselaer Polytechnic Institute (RPI) and nearby Troy, New York, with carrier-reported modeled coverage distributed through the FCC National Broadband Map. Field observations provide ground truth samples at particular places and times; coverage maps describe modeled availability and signal categories. They are different sources and must remain distinguishable.

## Quality and scope at a glance

| Item | Current contents |
|---|---|
| Named locations | 6, identified by `spot_id` S1-S6 |
| Collection time points | 2 visits/dates: October 15 and October 20, 2026; not simultaneous snapshots |
| Date range | 2026-10-15 through 2026-10-20; R03's October 15 date is assumed |
| Observation times | Eastern Time as labeled by the CSV's `*_et` columns; combine each clock time with its row date |
| Field records | 20: 12 T-Mobile and 8 Verizon |
| Record status | 14 Yes; 4 Review; 1 Signal only (failed test) |
| Completed cellular speed tests | 18 when Review records are retained; 14 when restricted to Yes |
| Spatial index | 6 distinct H3 resolution-9 indexes across all rows; not equivalent to 6 independent hexagonal sampling areas |
| Coordinate system | Decimal-degree latitude/longitude; coverage polygons use WGS 84 / EPSG:4326 |
| FCC version | Internal layer vintage code D25; T-Mobile layer suffix 04may2026, Verizon suffix 04aug2026; details below |
| Main gaps | R19 lacks a valid signal reading; R03 has an assumed date and hidden test details; R05 uses Wi-Fi; R20 is a failed test |
| Missing collection metadata | Phone models, app versions, numerical GPS accuracy and explicit standing-still/movement status are not columns in the supplied CSV |

T-Mobile has six records on each visit. Verizon has two records on visit 1 and six on visit 2, including the failed repeat at S6. This is not a complete carrier-by-location-by-visit panel. Verizon has no records for S1 on either visit or S4-S6 on visit 1. The files do not establish whether every absent combination was planned.

## Files in this release

Keep this README beside these five source files. Numbered suffixes are part of the attached filenames; they are not analytical version numbers.

| Filename | Contents | Provenance |
|---|---|---|
| `Cellular Readings - Combined T-Mobile + Verizon (Troy).csv` | 20 rows and 33 fields: field measurements, collection context, H3 identifiers, source references and quality flags | Field-observed values transcribed from phone screenshots, plus curator-assigned/derived metadata |
| `fcc_mobile_coverage_20260804_tmobile_4glte.gpkg` | New York T-Mobile LTE; 664,054 polygon records; provider 130403; technology 400; 5/1 Mbps threshold | Coverage-map data; modeled |
| `fcc_mobile_coverage_20260804_tmobile_5gnr.gpkg` | New York T-Mobile 5G-NR; 394,706 polygon records; provider 130403; technology 500; 35/3 Mbps threshold | Coverage-map data; modeled |
| ``fcc_mobile_coverage_20260804_verizon_5gnr.gpkg`` | New York Verizon 5G-NR; 289,378 polygon records; provider 131425; technology 500; 35/3 Mbps threshold | Coverage-map data; modeled |
| ``fcc_mobile_coverage_20260804_verizon_4glte.gpkg`` | New York Verizon LTE; 1,077,746 polygon records; provider 131425; technology 400; 5/1 Mbps threshold | Coverage-map data; modeled |

Each GeoPackage contains one feature layer. Polygon record counts describe each supplied statewide file, not counts of field locations or population covered. These are the corrected New York Verizon files; do not substitute the earlier Alabama files.

A separately created `metadata_plan.txt` describes intended collection metadata. It is a plan, not evidence that missing device, app or GPS-accuracy metadata have already been collected.

## FCC dates and version provenance

| Layer | Internal feature-layer name | GeoPackage `last_change` (UTC) | Embedded DBF update date |
|---|---|---|---|
| T-Mobile LTE | `bdc_36_130403_4GLTE_mobile_broadband_h3_D25_04may2026` | 2026-05-12T22:48:01.511Z | 2026-05-12 |
| T-Mobile 5G-NR | `bdc_36_130403_5GNR_35_3_mobile_broadband_h3_D25_04may2026` | 2026-05-12T23:51:15.114Z | 2026-05-12 |
| Verizon LTE | `bdc_36_131425_4GLTE_mobile_broadband_h3_D25_04aug2026` | 2026-08-05T22:19:19.299Z | 2026-08-05 |
| Verizon 5G-NR | `bdc_36_131425_5GNR_35_3_mobile_broadband_h3_D25_04aug2026` | 2026-08-05T22:31:06.727Z | 2026-08-05 |

The files explicitly identify vintage code D25. An exact human-readable availability "as of" date and the download-page "last updated" date are not recorded separately in their embedded metadata. D25 suggests a December 2025 reporting vintage; confirm the exact as-of date against the original FCC download record before treating that interpretation as verified. Do not substitute a filename date, layer suffix or GeoPackage modification timestamp for the availability date. In particular, the T-Mobile outer filenames contain 20260804 but their internal layers retain 04may2026.

## Connecting field readings to coverage maps

### Location crosswalk

| Location ID (`spot_id`) | Location label | H3 resolution-9 indexes present in the CSV |
|---|---|---|
| S1 | 377 Congress St | `892b89155c3ffff` |
| S2 | 1641 15th St (RPI) | `892b89155d3ffff`; R18 uses adjacent `892b89154a7ffff` |
| S3 | Sage Ave near 1145 (RPI) | `892b89154b7ffff` |
| S4 | 1401 Sage Ave (RPI) | `892b89154b7ffff` |
| S5 | Sunset Terr (RPI) | `892b8917373ffff` |
| S6 | 1990 Burdett Ave | `892b8917363ffff` |

`spot_id` is the project location ID. It groups repeat visits and carriers at a named spot; it is not an FCC location identifier. `reading_id` uniquely identifies an observation. `cell_id` identifies a cellular network cell, not a sampling location or an H3 hexagon.

## Field-reading CSV data dictionary

Origin labels: **FIELD** = observed/reported by the phone, GPS or test app; **CURATED** = assigned, derived or interpreted during compilation. Curated metadata are neither radio measurements nor coverage predictions. No CSV measurement column comes from a coverage map.

| Field | Origin | Meaning / units |
|---|---|---|
| `reading_id` | CURATED | Unique record identifier, R01-R20 |
| `spot_id` | CURATED | Repeat-location identifier, S1-S6 |
| `spot_name` | CURATED | Human-readable sampling-location label; see address caveats |
| `visit_no` | CURATED | Collection session: 1 = October 15; 2 = October 20 |
| `collected_by` | CURATED | Collector: Jamie or Isabella |
| `carrier` | FIELD | Carrier reported from the phone/dashboard; discrepancies are retained in notes |
| `date` | FIELD / CURATED exception | Collection date; R03 explicitly contains an assumed date |
| `signal_time_et` | FIELD | Signal screenshot clock time, HH:MM Eastern Time |
| `fcc_test_time_et` | FIELD | FCC app test time, HH:MM Eastern Time |
| `latitude` | FIELD | GPS latitude, decimal degrees north |
| `longitude` | FIELD | GPS longitude, decimal degrees; west is negative |
| `h3_index_res9` | CURATED / DERIVED | H3 resolution-9 spatial index associated with the recorded coordinates |
| `network_type` | FIELD / CURATED description | Connection classification based on app/dashboard evidence; preserve qualifications |
| `signal_tech_shown` | FIELD | Radio technology displayed for the signal values: LTE or 5G NR |
| `band` | FIELD | Displayed radio band, such as B12, B66, n41 or n77 |
| `bandwidth_mhz` | FIELD | Displayed channel bandwidth in MHz |
| `cell_id` | FIELD | Displayed cellular cell identifier; preserve as an identifier |
| `pci` | FIELD | Physical cell identity; not globally unique |
| `rsrp_dbm` | FIELD | Reference Signal Received Power in dBm; less-negative values indicate stronger received power |
| `rsrq_db` | FIELD | Reference Signal Received Quality in dB |
| `snr_sinr_db` | FIELD | Labeled SNR/SINR text in dB; may contain two streams; not one uniformly numeric variable |
| `fcc_test_id` | FIELD | FCC app test identifier; missing for R03 and R20 |
| `fcc_connected_type` | FIELD | App-reported connected network/type; not a coverage-map attribute |
| `fcc_test_type` | FIELD | App test category, Crowdsource or Challenge |
| `fcc_environment` | FIELD | Test environment reported by the app, usually OUTDOOR; R03 is not shown |
| `latency_ms` | FIELD | Test latency in milliseconds |
| `jitter_ms` | FIELD | Test jitter in milliseconds |
| `packet_loss_pct` | FIELD | Packet loss percentage, 0-100 |
| `download_mbps` | FIELD | Observed test download speed in Mbps; R20's zero denotes failure |
| `upload_mbps` | FIELD | Observed test upload speed in Mbps; R20's zero denotes failure |
| `include_in_comparison` | CURATED | Quality/use flag: Yes, Review, No or Signal only |
| `notes` | CURATED | Source qualifications, interpretations and data-quality issues |
| `source_screenshot` | CURATED provenance | Source reference such as Tab1 R1; combine with collector/carrier to disambiguate |

Blank cells mean missing/not recorded, not zero. Literal `not shown` means the source did not display that information. Do not remove the "(assumed)" qualifier from R03's date without retaining an assumption flag. Phone model, app version and numerical GPS accuracy are unknown from this CSV.

## Coverage-map data dictionary

Every field below is **MAP-SOURCED**. Signal/speed/environment fields describe modeled coverage; identifiers and geometry describe the map records. None are field-test results.

| Field | Origin | Meaning / units |
|---|---|---|
| `fid` | MAP identifier | GeoPackage feature ID, local to its layer |
| `geom` | MAP geometry | Coverage polygon in WGS 84 / EPSG:4326 |
| `frn` | MAP identifier | FCC Registration Number supplied with the map |
| `providerid` | MAP identifier | Carrier provider ID: 130403 or 131425 |
| `brandname` | MAP metadata | T-Mobile or Verizon |
| `technology` | MAP classification | 400 = 4G LTE; 500 = 5G-NR |
| `mindown` | MAP modeled threshold | Minimum download speed for the coverage category, Mbps: 5 LTE or 35 5G-NR |
| `minup` | MAP modeled threshold | Minimum upload speed for the coverage category, Mbps: 1 LTE or 3 5G-NR |
| `minsignal` | MAP modeled signal | Minimum modeled RSRP in dBm for these LTE/5G-NR layers; polygon attribute, not an exact phone observation |
| `environmnt` | MAP modeled condition | 0 = outdoor stationary only; 1 = outdoor stationary and in-vehicle mobile |
| `h3_res9_id` | MAP spatial identifier | H3 resolution-9 index used to connect with field spatial indexes |

For outdoor stationary comparisons, both environment codes are relevant. Code 1 does not mean indoor coverage. A mapped speed threshold is not the expected exact speed for each visit, and modeled signal categories are not exact point predictions.

## Flags, missed measurements and limitations

| Record(s) | Status / issue | Treatment |
|---|---|---|
| R02 | Review: both SINR streams report -20.0, potentially a floor value; LTE anchor shown | Retain the original values; assess signal comparability and show sensitivity to exclusion |
| R03 | Review: date assumed; test ID and environment hidden; LTE signal and additional 5G signal in notes | Do not treat its date as verified; select the technology corresponding to the signal being compared |
| R05 | No: speed test ran on RPI Wi-Fi | Exclude speeds from cellular coverage analysis; dashboard cellular signal has separate provenance |
| R18 | Review: test about three hours later than the paired T-Mobile test; app names RPI while dashboard identifies Verizon; adjacent H3 cell | Review carrier attribution and timing before paired comparisons |
| R19 | Review: no valid signal screenshot for S1; reused screenshot belonged to S2 | Signal RSRP, quality, band/cell details and signal time remain missing; a retake is needed |
| R20 | Signal only: failed Challenge test; no test ID, 100% packet loss, -121 dBm LTE signal | Retain as a failure/connectivity observation; do not summarize its zero speeds as a completed speed test |
| R01, R02, R03, R09, R17 | Dashboard signal is LTE although connection descriptions indicate 5G | Do not compare these LTE RSRP values directly with 5G modeled signal |
| R04 | Address note says 1108 Sage Ave, approximately 13 m from the named S3 point | Use row coordinates and retain the note |
| R10 | Dashboard had Wi-Fi icon; later speed test explicitly used mobile | Keep separate capture times and identify the test connection |
| R13, R15, R17 | Low download speeds or upload exceeding download noted despite usable signal | Preserve observations; unusual values are not automatically transcription errors |

Across the CSV, one RSRP value is missing, three bandwidth values are missing, 13 RSRQ values are missing, two test IDs are missing, and one latency/jitter pair is missing. Signal, location and speed screenshots were not always captured simultaneously. The small, uneven sample cannot establish overall carrier superiority or prove why a measured/model discrepancy occurred.

## Reference documentation

- FCC National Broadband Map: https://broadbandmap.fcc.gov/
- FCC coverage fields, modeled environments and thresholds: https://help.bdc.fcc.gov/hc/en-us/articles/6047425308187-Formatting-Mobile-Broadband-Availability-Coverage-Maps
- FCC download/version guidance: https://help.bdc.fcc.gov/hc/en-us/articles/43909220634651-How-to-Download-Mobile-Broadband-Coverage-Data-from-the-FCC-s-National-Broadband-Map-Step-by-Step-Instructions


