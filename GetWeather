# Writen for Python3 on a Raspberry Pi Zero W running Raspberry Pi OS and outputing to an InkyPHAT black and white display.
#
#
# This script requires the Pimori InkyPhat Python Library, as well as several other standard items.
#
import urllib.request
from inky import InkyPHAT
from bs4 import BeautifulSoup
from PIL import Image, ImageFont, ImageDraw
from font_source_sans_pro import SourceSansPro
import time

# I run this, via cron, every 5 minutes on a Raspberry Pi Zero W. I have this delay inserted because the WeeWx weather station 
# generates new page data every 5 minutes, which takes around 45 seconds to complete.
# This delay ensures that when I pull data from the WeeWx output, I'm getting the most recent, and not the data from the previous 5 minutes.
time.sleep(40)

#Insert the website address for your data pull. I have a special WeeWx template to generate a very simple, table based, site with just the required data.

content = urllib.request.urlopen('Your Simple, _TABLE BASED_ weather output')
read_content = content.read()
soup = BeautifulSoup(read_content,'html.parser')
tdAll = soup.find_all('td')

# We've now parsed each table element and can use those data to build the InkyPHAT display
#
# Bit of prep for the display

inky_display = InkyPHAT("black")
inky_display.set_border(inky_display.WHITE)
img = Image.new("P", (inky_display.WIDTH, inky_display.HEIGHT))
draw = ImageDraw.Draw(img)
font = ImageFont.truetype(SourceSansPro, 26)

# This is where we build up the message to insert. I use three lines on the display, temp, humidity, and the time stamp from the WeeWx output.
#
# '\n' Give you a new line.
#
# For the final line, the timestamp, I strip off all except HH:MM. I just want to be sure the data is current on the display.

message = "Temp: " + tdAll[0].text + "\n Humidity: " + tdAll[1].text + "\n Last Update: " + tdAll[2].text[11:17]

# This tells the script to place the data in the top left corner of the display.
w, h = font.getsize(message)
x = 1
y = 5

# And, finally, we draw the display.

draw.text((x, y), message, inky_display.BLACK, font)
inky_display.set_image(img)
inky_display.show()
