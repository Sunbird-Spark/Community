# Frontend migration

The Spark portal is a full rebuild of the Sunbird ED portal, not an upgrade. There is no upgrade path from the Angular 14 ED portal to the React 19 Spark portal.

If you have made custom modifications to the Sunbird ED portal codebase, those modifications need to be re-implemented in the new Spark portal codebase. The new portal uses the same API surface, so backend integrations can be carried over — but the frontend code is not compatible.
