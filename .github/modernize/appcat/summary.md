# Modernization Assessment Summary

**Target Azure Services**: Azure App Service, Azure Kubernetes Service, Azure Container Apps, Azure App Service Container

## Overall Statistics

**Total Applications**: 1

**Name: eShopLegacyWebForms**
- Mandatory: 2 issues
- Potential: 2 issues
- Optional: 3 issues

> **Severity Levels Explained:**
> - **Mandatory**: The issue has to be resolved for the migration to be successful.
> - **Potential**: This issue may be blocking in some situations but not in others. These issues should be reviewed to determine whether a change is required or not.
> - **Optional**: The issue discovered is real issue fixing which could improve the app after migration, however it is not blocking.

## Applications Profile

### Name: eShopLegacyWebForms
- **Frameworks**: .NETFramework,Version=v4.7.2
- **Languages**: C#
- **Build Tools**: MSBuild

**Key Findings**:
- **Mandatory Issues (2 locations)**:
  - <!--ruleid=Local.0004-->Logging to local or network paths detected (1 location found)
  - <!--ruleid=Identity.0002-->Windows authentication detected (1 location found)
- **Potential Issues (2 locations)**:
  - <!--ruleid=Connection.0001-->Connection string is detected (1 location found)
  - <!--ruleid=Scale.0002-->Session state stored in-proc or in local process is detected (1 location found)
- **Optional Issues (4 locations)**:
  - <!--ruleid=Scale.0001-->Static content detected (1 location found)
  - <!--ruleid=Database.0003-->System.Data.SqlClient dependency detected (1 location found)
  - <!--ruleid=Security.0002-->Connection strings without configuration builders detected (2 locations found)

## Next Steps

For comprehensive migration guidance and best practices, visit:
- [GitHub Copilot modernization](https://aka.ms/ghcp-appmod)
