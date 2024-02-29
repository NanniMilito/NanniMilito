import re
from instagram_private_api import Client

# Autenticati con il tuo account Instagram
username = 'tuo_username'
password = 'tuo_password'

api = Client(username, password)

# Cerca gli utenti con 30k follower
next_max_id = None
users = []

while True:
    result = api.user_search(q='', max_id=next_max_id)
    for user in result['users']:
        if user['follower_count'] > 30000:
            users.append(user)

    if not result['more_available']:
        break

    next_max_id = result['next_max_id']

# Filtra gli utenti con un dominio specifico nella bio
filtered_users = []

for user in users:
    bio = user['full_name'] + ' ' + user['bio']
    if re.search(r'@(gmail|libero|hotmail)\.com', bio):
        filtered_users.append(user)

# Invia un messaggio a ciascun utente filtrato
for user in filtered_users:
    api.direct_send(user['pk'], 'Ciao, come stai?')
