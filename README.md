!pip install vk_api
!pip3 install youtube-search-python

import os
import random
import json
from random import randrange
from vk_api.longpoll import VkLongPoll, VkEventType
from youtubesearchpython import VideosSearch
from urllib.request import Request, urlopen

TOKEN = 'vk1.a.hZkvACNAE1TP5swZ7TPwEjFwXbklIN8aHehj56t4xbcxN0Vffntbx2t6faQAG16tgsAQQm6iY6QlXu6Zz-acQg41xrlZ9Svc_zYKMmiuh7O0925t-bceGiqc72OYrNZaB9CfwOkXM1XodpIDtXN4WQ-wW5IbJbCfrOC3_ufVBeNIWgzA0ejye-CPjAi5fgbzp8X-T4Zp9DDf0EN3c3j6MQ'

vk_session = vk_api.VkApi(token=TOKEN)
vk = vk_session.get_api()
longpoll = VkLongPoll(vk_session)

def get_pokemons():
  req = Request('https://pokeapi.co/api/v2/pokemon', headers = {'User-Agent': 'mozilla/5.0'})
  response = urlopen(req)
  count = json.loads(response.read())['count']

  req =Request(f"https://pokeapi.co/api/v2/pokemon/?limit={count}", headers = {'User-Agent': 'mozilla/5.0'})
  response = urlopen(req)
  pokemons = json.loads(response.read())['results']
  return pokemons

def find_pokemons(name, pokemons):
  req = Request(pokemon['url'], headers={'User-Agent': 'mozilla/5.0'})
  response = urlopen(req)
  desc = json.loads(response.read())
  message = f"Имя: {desc['name']}\nРост: {desc['height']}\nВес: {desc['weight']}"
  return message

def find_video(name):
  link = VideosSearch(event.text, limit = 1).result()['result'][0]['link']
  return link

for event in longpoll.listen():
  if event.type == VkEventType.MESSAGE_NEW and event.to_me and event.text.lower():
    if event.from_user:
      command = event.text.split()[0].lower()
      event.text = event.text[len(command) +1:]
      if command == 'покемон':
        for pokemon in pokemons:
          if pokemon['name'] == event.text:
            message = find_pokemon(event.text, pokemons)
            vk.messages.send(user_id=event.user_id, message=message, random_id=randrange(1, 1000000))
            break
      elif command == 'ютюб':
        link = VideosSearch(event.text, limit = 1).result()['result'][0]['link']
        vk.messages.send(user_id=event.user_id, message=link, random_id=randrange(1, 1000000))
