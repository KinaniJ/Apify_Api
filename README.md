# Apify_Api
This is a Python function that will make an API call to apify.com.
def get_data_from_apify(api_token):
  """Gets data from the Apify API."""
  client = apify.Client(api_token)
  response = client.call('YOUR_API_SCRIPT_ID')
  return response.json()
  
It will then take the JSON response then remove the duplicates from it.
def remove_duplicates(data):
  """Removes duplicates from the data."""
  seen = set()
  new_data = []
  for item in data:
    if item not in seen:
      seen.add(item)
      new_data.append(item)
  return new_data

It thereafter will insert the data it into the MySQL database
def insert_data_into_mysql(data, cursor):
  """Inserts data into the MySQL database."""
  for item in data:
    cursor.execute('INSERT INTO table_name (column_names) VALUES (values)', item)

def main():
  """The main function."""
  api_token = 'YOUR_API_TOKEN'
  mysql_conn = mysql.connector.connect(
      host='localhost',
      user='root',
      password='',
      database='database_name')
  cursor = mysql_conn.cursor()

  data = get_data_from_apify(api_token)
  data = remove_duplicates(data)
  insert_data_into_mysql(data, cursor)
