# Measuring Nature balance: biodiveristy and disaster Indicator
My project is an interactive choropleth map application that visualizes global biodiversity and natural disaster data. Built using Python with PyQt5 and a local HTTP server, it displays the map in a desktop window through a web view widget. The project is a mix of both technical and humanitarian elements, aiming to highlight disparities in environmental impact and funding across different countries for protected areas globally.

| **Engineer** | **School** | **Area of Interest** | **Grade** |
|:--:|:--:|:--:|:--:|
| Cesar Cabral| KIPP NYC College prep high school | Enviromental Engineering | Incoming Senior

**Replace the BlueStamp logo below with an image of yourself and your completed project. Follow the guide [here](https://tomcam.github.io/least-github-pages/adding-images-github-pages-site.html) if you need help.**

![Headstone Image](photo1.png)
  
# Final Milestone


<iframe width="699" height="393" src="https://www.youtube.com/embed/jbHwYonGaEU" title="Cesar C Milestone 3" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
For my final project, I created a window displaying a choropleth map using a local HTTP server and a web view widget to showcase biodiversity and natural disasters. One of the biggest challenges I faced at BlueStamp was dealing with various technical difficulties, especially while connecting my Raspberry Pi to my computer, which required running many commands just to get small results. My project combined engineering and humanities, aiming to raise awareness about global disparities in resources. In the future, I hope to build more tech-based projects that highlight social and environmental issues in accessible, meaningful ways


# Second Milestone

<iframe width="853" height="480" src="https://www.youtube.com/embed/QFi2LPr4_sY" title="Cesar C Milestone 2" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

For my second milestone, I made a interactive choropleth map in the raspberrypi using Plotly Express and cleaned the dataset with Pandas. The map shows the percentage of protected areas by country and exports to HTML. A challenge I faced during this milestone was getting the terminal on my computer to connect properly to the Raspberrypi and for the final milestone, my goal is to display the map locally without opening a web browser.

# First Milestone!

<iframe width="853" height="480" src="https://www.youtube.com/embed/oZgps4pwm-o" title="Cesar C  Milestone 1" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>


My first milestone was essentially coding a computing screen coordinates system for my international project using a mathematical equation. However, the international space tracker couldn’t be completed because the necessary files on my computer weren’t showing up. As a result, I had to start a new project which is currently a  Biodiversity and Disaster Indicator. This project involves importing data that tracks a metric over time, followed by data processing to create a time series graph, with that being said, some of the challenges I faced while working on the computing screen coordinates were the amount of changes I had to make to meet my project’s requirements, for my second milestone I will have the choropleth map with a few other modifications.

# Code
Here's where you'll put your code. The syntax below places it into a block of code. Follow the guide [here]([url](https://www.markdownguide.org/extended-syntax/)) to learn how to customize it to your project needs. 

```c++
import os
os.environ["QTWEBENGINE_CHROMIUM_FLAGS"] = "--no-sandbox"  # Allow PyQtWebEngine to run as root

import sys
import pandas as pd
import plotly.express as px
import threading
from PyQt5.QtWidgets import QApplication, QMainWindow, QVBoxLayout, QWidget
from PyQt5.QtWebEngineWidgets import QWebEngineView
from PyQt5.QtCore import QUrl
from http.server import HTTPServer, SimpleHTTPRequestHandler
       

#Load and clean data``
df = pd.read_csv('Terrestrial_Marine protected areas.csv')
df.columns = ['CountryID', 'Country', 'LatestYear', 'ProtectedAreasPct']

#Generate and save the map
def create_map_html():
    fig = px.choropleth(
        df,
        locations="Country",
        locationmode='country names',
        hover_name="Country",
        hover_data=['LatestYear', 'ProtectedAreasPct'],
        color='ProtectedAreasPct',
        color_continuous_scale=px.colors.sequential.YlGn,
        title='Measuring Nature\'s Balance: Biodiversity and Disaster Indicators',
        range_color=(0, 60)
    )

    # This line disables external topojson fetch bc plotly kept trying to get an external map
    fig.update_geos(scope="world", showcountries=True)

    fig.update_layout(
        margin={"r": 0, "t": 30, "l": 0, "b": 0},
        coloraxis_colorbar=dict(
            title="% Protected",
            thickness=20,
            len=0.75,
            yanchor="middle",
            y=0.5
        )
    )

    output_file = "protected_areas_map.html"
    fig.write_html(output_file, include_plotlyjs='cdn', full_html=True)
    return os.path.abspath(output_file)


#Create PyQt window with embedded browser
class MapWindow(QMainWindow):
    def __init__(self, html_file):
        super().__init__()
        self.setWindowTitle("Protected Areas Map")
        self.setGeometry(100, 100, 1000, 700)

        self.browser = QWebEngineView()
        

        # Start a simple HTTP server in a background thread
        def start_http_server():
            server_address = ('', 8000)
            httpd = HTTPServer(server_address, SimpleHTTPRequestHandler)
            httpd.serve_forever()

        threading.Thread(target=start_http_server, daemon=True).start()

        self.browser.load(QUrl("http://localhost:8000/protected_areas_map.html"))

        layout = QVBoxLayout()
        layout.addWidget(self.browser)

        container = QWidget()
        container.setLayout(layout)
        self.setCentralWidget(container)

if __name__ == "__main__":
    app = QApplication(sys.argv)
    html_file = create_map_html()
    window = MapWindow(html_file)
    window.show()
    sys.exit(app.exec_())
```

# Bill of Materials
Here's where you'll list the parts in your project. To add more rows, just copy and paste the example rows below.
Don't forget to place the link of where to buy each component inside the quotation marks in the corresponding row after href =. Follow the guide [here]([url](https://www.markdownguide.org/extended-syntax/)) to learn how to customize this to your project needs. 

| **Part** | **Note** | **Price** | **Link** |
|:--:|:--:|:--:|:--:|
| Item Name | What the item is used for | $Price | <a href="https://www.amazon.com/Arduino-A000066-ARDUINO-UNO-R3/dp/B008GRTSV6/"> Link </a> |
| Item Name | What the item is used for | $Price | <a href="https://www.amazon.com/Arduino-A000066-ARDUINO-UNO-R3/dp/B008GRTSV6/"> Link </a> |
| Item Name | What the item is used for | $Price | <a href="https://www.amazon.com/Arduino-A000066-ARDUINO-UNO-R3/dp/B008GRTSV6/"> Link </a> |

# Other Resources/Examples
One of the best parts about Github is that you can view how other people set up their own work. Here are some past BSE portfolios that are awesome examples. You can view how they set up their portfolio, and you can view their index.md files to understand how they implemented different portfolio components.
- [Example 1](https://trashytuber.github.io/YimingJiaBlueStamp/)
- [Example 2](https://sviatil0.github.io/Sviatoslav_BSE/)
- [Example 3](https://arneshkumar.github.io/arneshbluestamp/)

To watch the BSE tutorial on how to create a portfolio, click here.
