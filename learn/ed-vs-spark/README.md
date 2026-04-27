# Ed Vs Spark

### The North Star of Spark

Before the breakdown, the single driving goal is clear: optimising for smaller scales without compromising DPI architectural principles, or on the ability to scale on demand. Spark will still scale for larger user bases if needed. Everything in Spark is in service of cost reduction, simplification, and modernization.



***

### WHAT STAYS THE SAME

#### Core Building Blocks (Retained as in SB Ed - not replaced)

| Component                                | Status                                                                                                                                   |
| ---------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------- |
| Sunbird Knowlg                           | ✅ Retained — merged but not replaced                                                                                                     |
| Sunbird Lern                             | ✅ Retained — merged but not replaced                                                                                                     |
| Sunbird inQuiry                          | ✅ Retained — merged into Knowlg service                                                                                                  |
| Sunbird RC v1.x                          | ✅ Explicitly kept as-is for Verifiable Credentials                                                                                       |
| Keycloak                                 | ✅ Retained for identity & auth (now with Yugabyte backend and upgraded to OIDC protocol enabling replacing keycloak with any other tool) |
| Kong API Gateway                         | ✅ Retained (upgraded, not replaced)                                                                                                      |
| Kafka                                    | ✅ Retained (upgraded to v4.x)                                                                                                            |
| Apache Flink                             | ✅ Retained — explicitly confirmed "No" to removing Flink                                                                                 |
| Content Players                          | ✅ All existing players reused (PDF, Video, H5P, ECML, EPUB)                                                                              |
| Collection & ECML Editors                | ✅ All editors reused with existing workflows                                                                                             |
| Telemetry Service                        | ✅ Retained as part of default install (even when Obsrv is add-on)                                                                        |
| QuML/inQuiry Player & Editor             | ✅ Retained                                                                                                                               |
| Certificate issuance and validation flow | ✅ Retained (Sunbird RC v1)                                                                                                               |
| Multi-tenancy via Channels               | ✅ Same logical multi-tenancy model                                                                                                       |
| Offline consumption (mobile)             | ✅ Retained, now explicitly in scope for Spark                                                                                            |

#### Completely new Android consumption app and portal

#### Core Functional Capabilities Retained

* Course creation, batch management, enrollment
* Certificate issuance & passbook
* Content creation workspace
* Content review workflows, including async publish workflow
* User profile & learning records
* Global search
* Anonymous content consumption
* RBAC & user management
* Notifications
* I18N / multilingual support
* Instrumentation & telemetry generation
