Changes from master
--------------------
Install with
```shell
git clone git@github.com:thenodon/grafana-backup-tool.git
python3 -m venv venv
. venv/bin/activate
pip install .
```

This version can only use GRAFANA_TOKEN so create Service account with admin permission token. 
Setup en like 
```shell
export GRAFANA_URL=http://localhost:3000
export GRAFANA_TOKEN=<YOUR GRAFANA TOKEN>
export PRETTY_PRINT=true
export VERIFY_SSL=false
```

Run backup 
```shell
grafana-backup save

```
This version of grafana-backup-tool include the following changes

## dashboardApi.py
diff --git a/grafana_backup/dashboardApi.py b/grafana_backup/dashboardApi.py
index 8fbdaf8..c470b68 100755
--- a/grafana_backup/dashboardApi.py
+++ b/grafana_backup/dashboardApi.py
@@ -13,7 +13,7 @@ def health_check(grafana_url, http_get_headers, verify_ssl, client_cert, debug):


def auth_check(grafana_url, http_get_headers, verify_ssl, client_cert, debug):
-    url = '{0}/api/auth/keys'.format(grafana_url)
+    url = '{0}/api/serviceaccounts/1/tokens'.format(grafana_url)
     print("\n[Pre-Check] grafana auth check: {0}".format(url))
     return send_grafana_get(url, http_get_headers, verify_ssl, client_cert, debug)

## save_dashboard_versions.py
diff --git a/grafana_backup/save_dashboard_versions.py b/grafana_backup/save_dashboard_versions.py
index 814f636..9735c83 100644
--- a/grafana_backup/save_dashboard_versions.py
+++ b/grafana_backup/save_dashboard_versions.py
@@ -54,6 +54,10 @@ def get_versions_and_save(dashboards, folder_path, log_file, grafana_url, http_g

def get_individual_versions(versions, folder_path, log_file, grafana_url, http_get_headers, verify_ssl, client_cert, debug, pretty_print):
file_path = folder_path + '/' + log_file
+    if versions:
+        versions = versions['versions']
+    else:
+        raise RuntimeError("versions, while getting individual versions broke(not present)")
  if versions:
  with open(u"{0}".format(file_path), 'w') as f:
  for version in versions: 



