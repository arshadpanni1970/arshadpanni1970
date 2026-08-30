# S/4HANA Fiori App Setup for `Z_CDS_PA9733_QRY`

Use this guide to activate and run a Fiori Elements analytical list/report app from your CDS query.

## 1) Activate CDS and generated OData service
1. Create/activate DDL source `Z_CDS_PA9733_QRY` with the content in `Z_CDS_PA9733_QRY.ddls.asddl`.
2. In transaction `/IWFND/MAINT_SERVICE`, add service:
   - Technical service name: `Z_CDS_PA9733_QRY_CDS`
   - System alias: your local S/4 alias (for example `LOCAL`)
3. Run transaction `/IWFND/GW_CLIENT` and test:
   - `/sap/opu/odata/sap/Z_CDS_PA9733_QRY_CDS/$metadata`

## 2) Create catalog and target mapping
In launchpad content manager (`/UI2/FLPCM_CUST`):
- Create catalog: `Z_PA9733_CATALOG`
- Create target mapping:
  - Semantic object: `ZPA9733`
  - Action: `display`
  - App type: `SAPUI5 Fiori App`
  - URL: `/sap/bc/ui5_ui5/sap/FIORI_EPM_APPS` (replace with your deployed app URL)

## 3) Generate Fiori Elements app in BAS/VS Code
Use Fiori tools "Create Project from Template":
- Template: **Analytical List Page** (or List Report)
- Data source: `Z_CDS_PA9733_QRY_CDS`
- Entity set: `Z_CDS_PA9733_QRY`
- Main OData service: `Z_CDS_PA9733_QRY_CDS`
- Add to launchpad with semantic object/action above.

## 4) Optional ABAP transaction launcher
If you want a classic t-code wrapper, create report `ZPA9733_FIORI_LAUNCH`:

```abap
REPORT zpa9733_fiori_launch.

DATA(lv_url) =
  '/sap/bc/ui2/flp#ZPA9733-display?BankArea=&/Z_CDS_PA9733_QRY'.

cl_abap_browser=>show_url(
  EXPORTING
    url = lv_url ).
```

## 5) Authorizations
Even with `#NOT_REQUIRED` in CDS, ensure user has:
- Gateway runtime roles
- Launchpad catalog/group assignment
- OData service authorization in your landscape policy
