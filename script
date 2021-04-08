# See API docs at http://nexus-url/#admin/system/api

import requests

USERNAME = 'user'
PASSWORD = 'password'
NEXUS_BASE_URL = 'http://nexus-url/service/rest/beta'
REPOSITORY = 'some-project-snapshots'
GROUP_FILTER = 'com.some.group'


def get_components_response(params):
  return requests.get(f'{NEXUS_BASE_URL}/components', auth=(USERNAME, PASSWORD), params=params).json()


def delete_component(component):
  response = requests.delete(f'{NEXUS_BASE_URL}/components/{component["id"]}', auth=(USERNAME, PASSWORD))
  response.raise_for_status() # or print(response.status_code)


response = get_components_response({'repository': REPOSITORY})
components = response['items']

while response['continuationToken']:
  response = get_components_response({'repository': REPOSITORY, 'continuationToken': response['continuationToken']})
  components.extend(response['items'])  

filtered = [component for component in components if component['group'] == GROUP_FILTER]

for component in filtered:
  print('Deleting', component['name'], component['version'])
  delete_component(component)
