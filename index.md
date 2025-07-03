# Measuring Nature balance: biodiveristy and disaster Indicator
This project is an interactive choropleth map application designed to visualize global biodiversity metrics and natural disaster data, with a particular focus on protected areas and environmental impact. The goal is to highlight disparities in ecological vulnerability and conservation funding across different countries, blending both engineering and humanities.

| **Engineer** | **School** | **Area of Interest** | **Grade** |
|:--:|:--:|:--:|:--:|
| Cesar Cabral| KIPP NYC College prep high school | Enviromental Engineering | Incoming Senior

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
       

``
df = pd.read_csv('Terrestrial_Marine protected areas.csv')
df.columns = ['CountryID', 'Country', 'LatestYear', 'ProtectedAreasPct']

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


class MapWindow(QMainWindow):
    def __init__(self, html_file):
        super().__init__()
        self.setWindowTitle("Protected Areas Map")
        self.setGeometry(100, 100, 1000, 700)

        self.browser = QWebEngineView()
        

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

| **Part** | **Note** | **Price** | **Link** |
|:--:|:--:|:--:|:--:|
| Raspberry Pi Kit | Portable computer that controls electronic components | $103 | <a href="https://a.co/d/7lLaQ39"> Link </a> |
| 7-inch LCD Display | Used to show the Raspberrypi interface | $45 | <a href="https://www.amazon.com/dp/B09XKC53NH?ref_=cm_sw_r_cp_ud_dp_TX31X387PQ9KQDQS312H_2"> Link </a> |
| Mouse & Keyboard | Display navigation | $20 | <a href="https://a.co/d/7DeSDwC"> Link </a> |
| USB C to USB Adapter | Used to connect LCD display to computer | $10 | <a href="https://a.co/d/i34y1F2"> Link </a> |
