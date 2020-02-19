## Sea of mountains

### Frontend

#### QR code
the `QR` code point to dynamic generated entry page with:

* An email field
* A text field to apply the code. This code is a random number returned by the `backend`
* We need a way to post this data to some REST endpoint


#### Main Page
The main page is a simple, it contains some header with OpenBass media and main container.
The game starts with empty main container ( the body ) who is a matrix with dimensions of 240x240.
That means we having 30 codes per row, each code has a length by 8 chars from class **[0-9-A-Z]**

The total number of codes are 900.

When they break the code all users who are participated will receive an email with additional information.

#### How things works

1. The QR code.
The usual step is to pick a flayer, next scanning the QR code you are visiting `openbass.life` web page.
It is a unique dynamically generated page that contains the fields mentions above. The user have to fill these
fields and is ready to go.

2. The adventure request
Few or more requests that comes from the QR codes will point to different page called - an adventure page.
There the user will find an invitation to continue its journey. That meaning we haven't the fields from the section above, instead we having a simple text with GPS coordinates. These GPS coordinates are dynamic and will be not more than 30-40 pairs. On every place that each one pair follow they will find an OpenBass sticker with another QR code.
This guys that play this part of the game are the actual people who win and break the code. The new QR code will point
to another simple page that has a text with a wining message some bonus maybe and the code that the user will apply,
so could be the same page as the regular QR but with the winning text ( `?winning=True` )

3. The Joke Page
So might some of the incoming QR request will point to page that will said some random joke,
and they will not get a code, instead for the next 48h this IP address will be banned.

4. Main page
This page will showing the progress of code breaking, for this purpose we having this matrix.
The `FE` will receive the data as initial request and could be rendered in way to represent real time updates.
Each code will have `X` and `Y` index, so we could generate a random matrix.


### BackEnd
TODO
