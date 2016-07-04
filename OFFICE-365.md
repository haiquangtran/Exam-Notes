# Office 365 Developer Overview
- Focusing on Business Solutions into Office 365 products

## Apps for SharePoint

### The App Model
- Contextual Apps
- Target multiple platforms
- SharePoint apps do not "live" on the SharePoint server
- Custom code executes in the client, cloud or on-premises.
- Apps are granted permissions to SharePoint via OAuth
- Apps communicate with REST/CSOM Api
- Acquire apps via centralized location
  - App catalog
  - Public store (via submission process)
  - APIs for manual deployment

## SharePoint Building Blocks
- Lists/libraries
- Web parts
- Site Columns
- Content types
- Remote event receivers
- Workflows

## Provider vs SharePoint hosted
- **Provider hosted apps**
  - Preferred model for almost all types of apps
  - Full power of web - choose your infrastructure and technology
  - May require your own hosting
  - May require your own handling of multitenancy and permission management
- **Provider hosted apps/SharePoint hosted apps**
  - Good for smaller apps and resource storage
  - SharePoint-based; no server-side code
  - Automatically hosted in SharePoint
  - Inherent multienancy and isolation

- What's the difference between Office 365 and SharePoint?

