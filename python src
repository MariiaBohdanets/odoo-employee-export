import ssl
import os
import xmlrpc.client
import pandas as pd

# --- připojení ---
odoo_url = os.environ["ODOO_URL"]
odoo_db = os.environ["ODOO_DB"]
odoo_user = os.environ["ODOO_USER"]
odoo_password = os.environ["ODOO_PASSWORD"]

ssl_context = ssl._create_unverified_context()

common = xmlrpc.client.ServerProxy(
    f"{odoo_url}/xmlrpc/2/common",
    context=ssl_context
)

uid = common.authenticate(
    odoo_db,
    odoo_user,
    odoo_password,
    {}
)

models = xmlrpc.client.ServerProxy(
    f"{odoo_url}/xmlrpc/2/object",
    context=ssl_context
)

# --- konfigurace dotazu ---
fields = [
    "user_id",
    "name",
    "department_id",
    "parent_id"
]

domain = [
    ("department_id", "!=", "EXT")
]

# --- načtení dat ---
data = models.execute_kw(
    odoo_db,
    uid,
    odoo_password,
    "hr.employee.public",
    "search_read",
    [domain],
    {"fields": fields}
)

df = pd.DataFrame(data)

# --- testovací výstup ---
df.to_csv("employees_public_latest.csv", index=False)

df
