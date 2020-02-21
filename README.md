![open bass](https://github.com/openbass/seaofmountains/raw/master/static/images/header-1280x300.jpg)

## Sea of mountains / Draft

### Frontend

#### QR code

the `QR` code point to dynamic generated entry page with:

- An nickname field
- A text field to apply the code. This code is a random number returned by the `backend`
- We need a way to post this data to some REST endpoint

#### Main Page

The main page is a simple, it contains some header with OpenBass media and main container.
The game starts with empty main container ( the body ) who is a matrix with dimensions of 240x240.
That means we having 30 codes per row, each code has a length by 8 chars from class **[0-9-A-Z]**

The total number of codes are 900.

###### TODO

When they break the code all user pages will show up a link

#### How things works

**NOTES**
Every request that come from QR code will create a unique user page. The user must be encouraged to save
the link. The user need need agreed or not about the cookie polices ( we do not play without cookie, Pooh )

1. The QR code.
   The usual step is to pick a flayer, next scanning the QR code you are visiting `openbass.life` web page.
   It is a unique dynamically generated page that contains the fields mentions above. The user have to fill these
   fields and is ready to go.

2. The adventure request.
   Few or more requests that comes from the QR codes will point to different page called - an adventure page.
   There the user will find an invitation to continue its journey. That meaning we haven't the fields from the section above,
   instead we having a simple text with GPS coordinates. These GPS coordinates are dynamic and will be not more than 30-40 pairs.
   On every place that each one pair follow they will find an OpenBass sticker with another QR code.
   This guys that play this part of the game are the actual people who win and break the code. The new QR code will point
   to another simple page that has a text with a wining message some bonus maybe and the code that the user will apply,
   so could be the same page as the regular QR but with the winning text ( `?winning=True` )

3. The Joke Page.
   So might some of the incoming QR request will point to page that will said some random joke,
   and they will not get a code, instead for the next 48h this IP address will be banned.

4. Main page.
   This page will showing the progress of code breaking, for this purpose we having this matrix.
   The `FE` will receive the data as initial request and could be rendered in way to represent real time updates.
   Each code will have `X` and `Y` index, so we could generate a random matrix.

### BackEnd

#### Requirements

The `Backend` will be written in Python 3.7. The `HTML` and the `REST` endpoints will be served
by `Sanic` it is a modern Python `asyncio` framework. For session cache and data storage we going to use Redis.
All data stored in Redis has a specific TTL, when expire the data is auto-deleted.

If the `OS` it is not shipped with Python 3.7 here is the steps to install it

1. Install `pyenv` - https://github.com/pyenv/pyenv
2. Installation of Python 3.7

```shell
$ pyenv install 3.7.4
```

3. Create a `virtualenv` for the project

```shell
$ pyenv virtualenv 3.7.4 sea374
```

The game has an expire date, after that the site and all pages will gone.
We are going to set a cookie that will help to connect user device to `UUID` and ensure the link isn't shared.

#### User Endpoints

1. The QR endpoint.
   This endpoint is hit by the user after scanning of the QR code. This endpoint will generate a unique `UUID`.
   The `UUID` is a user_id in our case and when is generated will redirect to the next endpoint.

2. User Home Page.
   URI `GET /user/UUID`

3. Home.
   The root or the home page points to
   URI: `GET /`

4. The adventure page.
   URI: `GET /user/UUID/adventure`

5. The Joke page.
   URI: `GET /user/UUID/badluck`

#### REST Endpoints

1. The init endpoint.
   This endpoint is used for initial request. It will return the current state of the code including the progress.
   URI: `GET /api/init`

2. Get a user code endpoint.
   Return a part of the code.
   URI: `GET /api/UUID/code/`

3. QR Endpoint.
   This endpoint is used to save the user request with the `nickname` and the `code` returned by the `Backend`.
   URI: `POST /api/breakthecode`

   The body of the request is the follow:

```javascript
request = {
  nickname: "Kockata",
  code: 12345678
};
```

4. An adventure endpoint
   Here we need to fetch the GPS coordinates for the next step of the game.
   URI: `POST /api/adventure`

### Installation

#### Backend

```shell
$ git clone https://github.com/openbass/seaofmountains
$ cd seaofmountains
$ pyenv activate sea374
$ pip install -r requirements.txt
$ python app.py
```
