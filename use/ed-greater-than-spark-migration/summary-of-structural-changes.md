# Summary of Structural Changes

| Services           | Sunbird ED                    | Sunbird Spark                                    | Impact                                         |
| ------------------ | ----------------------------- | ------------------------------------------------ | ---------------------------------------------- |
| Backend services   | 8+ separate microservices     | 2 merged services (knowlg-service, lern-service) | API endpoints unchanged; deployment simplified |
| Primary database   | Cassandra + PostgreSQL        | YugabyteDB                                       | Data migration required                        |
| Analytics pipeline | Obsrv mandatory               | Obsrv optional add-on                            | Must explicitly enable if needed               |
| Flink jobs         | \~20+ jobs running by default | Pruned set — \~8 essential jobs                  | Removed jobs must not be re-deployed           |
| Frontend           | Angular 14 portal             | React 19 portal (full rebuild)                   | Not an upgrade — separate deployment           |
| DIAL/QR codes      | Default ON                    | Optional add-on                                  | Must explicitly enable if needed               |
| Discussion Forum   | Default ON                    | Optional add-on                                  | Must explicitly enable if needed               |
| Groups             | Default ON                    | Optional add-on                                  | Must explicitly enable if needed               |
