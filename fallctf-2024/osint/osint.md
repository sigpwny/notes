---
title: OSINT
---
# **A Guide to OSINT**

## **Introduction**
OSINT, short for **O**pen **S**ource **Int**elligence, focuses on gathering and analyzing publicly available information from various online sources. This information may range from personal identifiable information (PII) to social media profiles, public records, or even metadata. While OSINT can be a powerful tool, it is important to approach it with caution to ensure that ethical boundaries and privacy laws are respected.

This guide explores various OSINT tools, techniques, and methodologies that will assist you in solving Fall CTF OSINT challenges. 

## **Types of Data Sources**
Information gathered from OSINT can be found in the form of online and offline sources. Online sources refer to publicly accessible information through the internet. Examples include social media, blogs, public web pages, forums, etc. Offline sources refer to data that may not be directly accessible online (ex: downloaded files).

## **OSINT Techniques**
**1. Context Clues in Photos**

Photos can reveal critical information if you know where to look. By analyzing clues such as signs, geographical features, architecture, etc., you can determine the location of the photo or gain context about a person/event. For example, kanji characters on a sign may indicate Japan. Unique architecture/objects and street signs can also point you in the right direction. 

**2. Using Social Media**

Social media is a treasure trove of information. Individuals often share an abundance of personal information, including their location, photos, and connections to other people. Some starting points:

* **Cross-platform investigation:** Individuals who are often active on one platform can be found another (ex: Instagram and X). In particular, [Sherlock](https://github.com/sherlock-project/sherlock) is a popular tool for finding social media platforms that a particular username is on.
* **Tags:** A user might inadvertently disclose their location or interesting information through the use of hashtags. 
* **Interactions:** Interactions can include comments, likes, etc. Keep an eye out for any interactions, especially ones that may look out of place. 

**3. Utilizing Search Engines**

Search engines are incredibly important in OSINT. They enable you to search for websites, keywords, and even specific file types. Optimizing your search engine input will allow you to refine your results and find what you are looking for substantially quicker. Starting points include:
* **Keyword Search:** Using relevant keywords or specific quotes can help locate information more precisely. You can also combine keywords with Boolean operators such as "AND", "OR", and "NOT". 
* **Site-Specific Search:** If you know the site you are interested in finding information on, you can use the site operator. 

    Example: ```site:github.com "Username"```
* **Image Search:** You can upload an image to image search engines to see if an image has been posted online before (ex: [TinEye](https://tineye.com/), [Google Lens](https://lens.google/)). 

**4. Metadata and File Analysis**

Metadata refers to the hidden information embedded within files, such as images, documents, and videos. Metadata can provide insights about a file that are not immediately obvious, making it valuable in OSINT.

Metadata can reveal:
- File Name
- File Type
- Camera Information (for images)
- Date and Time of file creation
- GPS Location
- Author of file
- **And more!**

In the context of images, a specific type of metadata known as term *EXIF* (Exchangeable Image Format) is often included. EXIF data can reveal information about the camera/smartphone used to capture the image, the alias of the author, GPS coordinates, and more.

There are tools online that can be used to read this data and edit it. [ExifTool](https://exiftool.org/) is a commonly used, powerful command-line application for reading, writing, and extracting metadata information. 

**5. Geolocation Services**

Geolocation is the process of determining the physical location of an object. In Fall CTF, the object will typically be a photograph. This can be very tricky, so it is helpful to combine different techniques in order to verify your findings and reduce false positives.

**Common techniques used in geolocation include:**
- **Image analysis:** Identify landmarks, street signs, or other recognizable features in the image. 
- **Satellite and map data:** Make use of maps (ex: Google Maps) and satellite imagery to match locations. [Overpass Turbo](https://overpass-turbo.eu/) is a tool that queries OpenStreetMap (OSM) data, allowing you to search for specific types of geographical data (ex: road signs, buildings) and visualize them on the map. 

    **For example**, the following Overpass Turbo query will retrieve nodes (points) in OSM that are tagged as drinking water locations:

    ```
    node
        [amenity=drinking_water]
        ({{bbox}});
    out;
    ```

    There are many different queries you can construct! Play around with the "Load" and "Query Wizard" features on the [OverPass Turbo website](https://overpass-turbo.eu/).

**6. Web Archive/History**

The internet is constantly evolving, with websites and users frequently updating the content they display. For example, content may not always be displayed on the main page of a website. Therefore, it may be helpful to check if the "Archive" page (if it exists) of a website contains anything of interest. 

Additionally, examples of commonly used web archiving tools that allow you to examine snapshots of a website include [Wayback Machine](https://web.archive.org/) and [archive.today](https://archive.ph/). 

## **General Advice**
OSINT requires patience, precision and creativity. Remember that even minor details can hold importance - pay attention to these clues so you can piece together the bigger picture. Experiment with different tools (metadata extraction, social media platforms, search engine filters) for different tasks so you can uncover new insights. Good luck!
