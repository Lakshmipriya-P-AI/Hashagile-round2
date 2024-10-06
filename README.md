# Hashagile-round2
# Task 1
```
import pandas as pd

data = pd.read_csv('employee.csv')


data.to_json('employee.json', orient='records', lines=True)

curl -X PUT "http://localhost:9200/employees" -H 'Content-Type: application/json' -d '{
  "mappings": {
    "properties": {
      "EmployeeID": { "type": "integer" },
      "FirstName": { "type": "text" },
      "LastName": { "type": "text" },
      "Email": { "type": "text" },
      "Phone": { "type": "text" },
      "JobTitle": { "type": "text" },
      "Salary": { "type": "float" },
      "Department": { "type": "text" },
      "DateHired": { "type": "date" }
    }
  }
}'

curl -X POST "http://localhost:9200/employees/_bulk" -H 'Content-Type: application/json' --data-binary "@employee.json"

curl -X GET "http://localhost:9200/employees/_search?pretty"

curl -X PUT "http://localhost:9200/employees" -H 'Content-Type: application/json' -d '{
  "mappings": {
    "properties": {
      "EmployeeID": { "type": "integer" },
      "FirstName": { "type": "text" },
      "LastName": { "type": "text" },
      "Email": { "type": "text" },
      "Phone": { "type": "text" },
      "JobTitle": { "type": "text" },
      "Salary": { "type": "float" },
      "Department": { "type": "text" },
      "DateHired": { "type": "date" }
    }
  }
}'

curl -X POST "http://localhost:9200/employees/_bulk" -H 'Content-Type: application/json' --data-binary "@employee.json"

curl -X GET "http://localhost:9200/employees/_search?pretty"
```

![image](https://github.com/user-attachments/assets/09854d8d-4487-402a-a081-f7a6312012f0)
![image](https://github.com/user-attachments/assets/b1ffeddd-08c6-47bc-abf1-9c46c0be6bcc)

# Task 2

## program code:
```
employees = [
    {'EmployeeID':'E01','Name':'Alice','Department':'HR','Gender':'Female'},
    {'EmployeeID':'E02','Name':'Max','Department':'IT','Gender':'Male'},
    {'EmployeeID':'E03','Name':'Kevin','Department':'IT','Gender':'Male'},
    {'EmployeeID':'E04','Name':'David','Department':'Finance','Gender':'Male'},
    {'EmployeeID':'E05','Name':'Mikasa','Department':'HR','Gender':'Female'}
]
collections = {}

def createCollection(p_collection_name):
    collections[p_collection_name] = []
    print(f"Collection '{p_collection_name}' created.")


def indexData(p_collection_name, p_exclude_column):
    if p_collection_name not in collections:
        print(f"Collection '{p_collection_name}' does not exist.")
        return
    
    for emp in employees:
        indexed_emp = {key: value for key, value in emp.items() if key != p_exclude_column}
        collections[p_collection_name].append(indexed_emp)
    print (f"Data '{p_collection_name}', with excluded'{p_exclude_column}'.")

def searchByColumn(p_collection_name, p_column_name, p_column_value):
    if p_collection_name not in collections:
        print(f"Collection '{p_collection_name}' does not exist.")
        return
    
    results = [emp for emp in collections[p_collection_name] if emp.get(p_column_name) == p_column_value]
    if results:
        print (f" Search Results in '{p_collection_name}' for {p_column_name} = {p_column_value}: {results}")
    else:
        print (f" No results in '{p_collection_name}' for {p_column_name} = {p_column_value}")

def getEmpCount(p_collection_name):
    if p_collection_name not in collections:
        print(f"Collection '{p_collection_name}' does not exist.")
        return
    
    count = len(collections[p_collection_name])
    print(f"Employee count in '{p_collection_name}': {count}")
    return count

def delEmpById(p_collection_name, p_employee_id):
    if p_collection_name not in collections:
        print(f"Collection '{p_collection_name}' does not exist.")
        return
    
    original_count = len(collections[p_collection_name])
    collections[p_collection_name] = [emp for emp in collections[p_collection_name] if emp.get('EmployeeID') != p_employee_id]
    new_count = len(collections[p_collection_name])
    
    if original_count == new_count:
        print(f" No employee with ID '{p_employee_id}' found in '{p_collection_name}'.")
    else:
        print(f" Employee with ID '{p_employee_id}' deleted from '{p_collection_name}'.")

def getDepFacet(p_collection_name):
    if p_collection_name not in collections:
        print(f"Collection '{p_collection_name}' does not exist.")
        return
    
    dep_count = {}
    for emp in collections[p_collection_name]:
        dep = emp.get('Department', 'Unknown')
        dep_count[dep] = dep_count.get(dep, 0) + 1
    
    print(f" Department count in '{p_collection_name}': {dep_count}")
    return dep_count


v_nameCollection = 'Hash_Lakshmipriya'
v_phoneCollection = 'Hash_2096'   

createCollection(v_nameCollection)
createCollection(v_phoneCollection)

getEmpCount(v_nameCollection)
indexData(v_nameCollection, 'Department')
indexData(v_phoneCollection, 'Gender')

delEmpById(v_nameCollection, 'E02003')
getEmpCount(v_nameCollection)

searchByColumn(v_nameCollection, 'Department', 'IT')
searchByColumn(v_nameCollection, 'Gender', 'Male')
searchByColumn(v_phoneCollection, 'Department', 'IT')

getDepFacet(v_nameCollection)
getDepFacet(v_phoneCollection)
```

## Output:

![image](https://github.com/user-attachments/assets/4cf33ba5-69f1-4953-815c-47307cb2e0b2)

