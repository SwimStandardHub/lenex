# Lenex 3.1 - Technical documentation
An international data exchange format for swimming.

## 1. General
A "Lenex" file is a XML file with some additional constraints, which cannot be defined with an XSD schema. Because of this, and to give programmers an easier to read document with additional information on the format, we have put together this "non-standard" kind of documentation for you.

## 2 . Lenex files
A Lenex file is a XML file with the extension **.lef**. Usually, Lenex files are compressed (in the ZIP file format) and are labeled with the extension **.lxf**. A Lenex file can contain all kinds of data at the same time. However, when exchanging data, the following files with a subset of possible objects are commonly used:
  * **Invitation:** An invitation file contains general information, the schedule and the event structure of one meet. Additionally, it could be necessary or helpful to add time standards and/or qualification times, which are important for the meet.
  * **Entries:** An entry file contains the entries for one meet. One file might contain the entries of one club only, or it can contain all entries of all clubs.
  * **Results:** A result file contains the results of one meet. Normally, it contains all results of all clubs, but it is possible to split the results for each club into a separate file.
  * **Records:** A record file contains one or more list(s) of records.
  * **Time standards:** A time standards file may contain different kinds of time standards and/or qualification times. It might make sense to store time standards in separate files, if they are independent of meets (e.g. Olympic A and B time standards). If the time standards are bound to a certain meet, they should be included in the invitation file of that meet.

Generally the items in a Lenex file fall into three categories:
  * **Elements:** An element contains any number of child objects and attributes.
  * **Collections:** A collection is an element that contains elements of one type only. By default, the collection will be given the plural name of the element name it contains (e.g. SESSIONS contains SESSION objects). In most cases, the objects contained in a collection have at least one required attribute for identification purposes (e.g. attribute "distance" for element SPLIT).
  * **Attributes:** Attributes contain data in one of the basic Lenex data formats. The recognized formats are documented in the chapter "Lenex data types". Attributes can be attached to objects or to collections.

## 3. Lenex tree
This is an overview of the Lenex structure. Subchapter 3.1 is a tree with the most important elements. The other subchapters describe some of the sub trees in more details. To get the full information about all elements that are allowed or required, please refer to the chapter with the "Lenex element documentation".

### 3.1. Tree overview
The following tree shows the most important elements in a Lenex tree.
```
<LENEX> <!-- The root of a Lenex file. -->
	<CONSTRUCTOR /> <!-- Information about the creator of the file. -->
    <MEETS>
    	<MEET> <!-- The root for a meet sub tree. -->
        	<SESSIONS /> <!-- The schedule and event details of a meet. -->
            <CLUBS>
            	<CLUB> <!-- All data of one club at the meet. -->
                	<ATHLETES />
                    <RELAYS />
                    <OFFICIALS />
                </CLUB>
            </CLUBS>
        </MEET>
    </MEETS>
    <RECORDLISTS /> <!-- The root for the record lists sub tree. -->
    <TIMESTANDARDLISTS /> <!-- The root for the time standard lists sub tree. -->
</LENEX>
```

### 3.2 Sub tree `<SESSIONS />`
The SESSIONS sub tree describes the entire event structure with prelims and final events and the age groups used for the result lists.
```
<SESSIONS>
	<SESSION> <!-- Data of one session with all its events.  -->
    	<POOL />
        <EVENTS>
        	<EVENT> <!-- Description of one event/round. -->
            	<AGEGROUPS>
                	<AGEGROUP> <!-- Details of one age group.  -->
                    	<RANKINGS>
                        	<RANKING /> <!-- Details for ranking with reference to result elements.  -->
                        </RANKINGS>
                    </AGEGROUP>
                </AGEGROUPS>
                <HEATS>
                	<HEAT /> <!-- Details of one heat (number, starttime). -->
                </HEATS>
                <SWIMSTYLE />
                <TIMESTANDARDREFS>
                	<TIMESTANDARDREF /> <!-- Reference to a timestandard list. -->
                </TIMESTANDARDREFS>
            </EVENT>
        </EVENTS>
        <JUDGES>
        	<JUDGE /> <!-- Details about judges for a session. -->
        </JUDGES>
    </SESSION>
</SESSIONS>
```

### 3.3. Sub tree `<ATHLETES \>`
The ATHLETES sub tree contains all athletes of one club with their entries and/or results.
```
<ATHLETES>
	<ATHLETE> <!-- Data of one athlete. -->
    	<ENTRIES>
        	<ENTRY> <!-- Entry for one event/round. -->
            	<MEETINFO />
            </ENTRY>
        </ENTRIES>
        <RESULTS>
        	<RESULT> <!-- Result for one event/round. -->
            	<SPLITS />
            </RESULT>
        </RESULTS>
    </ATHLETE>
</ATHLETES>
```

### 3.4. Sub tree `<RELAYS />`
The RELAYS part is used to describe relay entries and results of one club. The relay swimmers are not stored directly in this tree. A unique id is stored in the tree in order to reference an athlete in the ATHLETES sub tree.
```
<RELAYS>
	<RELAY>
    	<ENTRIES>
        	<ENTRY> <!-- Entry for one event/round. -->
            	<RELAYPOSITIONS>
                	<RELAYPOSITION>
                    	<MEETINFO />
                    </RELAYPOSITION>
                </RELAYPOSITIONS>
            	<MEETINFO />
            </ENTRY>
        </ENTRIES>
        <RESULTS>
        	<RESULT> <!-- Result for one event/round. -->
            	<RELAYPOSITIONS>
                	<RELAYPOSITION />
                </RELAYPOSITIONS>
            	<SPLITS />
            </RESULT>
        </RESULTS>
    </RELAY>
</RELAYS>
```

### 3.5. Sub tree `<RECORDLISTS />`
The sub tree RECORDLISTS is used to define all kind of records. One record list contains all records of a specific type (e.g. world records), gender and pool length (course). In this sub tree, the information about the athletes is represented by means of complete ATHLETE objects, and not just as a reference to some other sub tree.
```
<RECORDLISTS>
	<RECORDLIST> <!-- Data of one record list (type, gender, course). -->
    	<AGEGROUP />
        <RECORDS>
        	<RECORD> <!-- Data of one record (individual or relay). -->
            	<SWIMSTYLE />
                <ATHLETE />
                <RELAY>
                	<RELAYPOSITIONS>
                    	<RELAYPOSITION>
                        	<ATHLETE />
                        </RELAYPOSITION>
                    </RELAYPOSITIONS>
                </RELAY>
                <MEETINFO />
                <SPLITS />
            </RECORD>
        </RECORDS>
    </RECORDLIST>
</RECORDLISTS>
```

### 3.6. Sub tree `<TIMESTANDARDLISTS />`
The sub tree TIMESTANDARDLISTS is used to define time standards and qualification times.
```
<TIMESTANDARDLISTS>
	<TIMESTANDARDLIST> <!-- Time standards (type, gender, course). -->
    	<TIMESTANDARDS>
        	<TIMESTANDARD> <!-- Data of one time standard / qual. time. -->
            	<SWIMSTYLE />
            </TIMESTANDARD>
        </TIMESTANDARDS>
    </TIMESTANDARDLIST>
</TIMESTANDARDLISTS>
```

## 4. Lenex element documentation
The following list is alphabetically ordered and describes the meaning and content of every element in a Lenex tree. Element names are in uppercase. Attribute names are in lowercase. The basic data types are described in chapter "Lenex data types". Elements can appear in different ways:
* **Normal (-):** Zero or one instance of an attribute/element is allowed.
* **Required (r):** Exactly one instance of an attribute/element is required.
* **Multiple (m):** There can be any number of instances of an elements (including zero). An attribute is always allowed once in maximum per element.

Every element or collection can have an attribute with the name "[elementname]id" (e.g. "athleteid" for the element ATHLETE). For some objects, this attribute is mandatory, because it is used to build relationships between elements in different sub trees. Attributes have to be unique over all instances of element type.

### Element `<AGEDATE />`
The AGEDATE is the date used to calculate the age of an athlete.
<table width=100%>
    <tr>
        <th width=25%>Element/Attribute</th>
        <th width=5%>Type</th>
        <th width=5%></th>
        <th width=65%>Remarks</th>
    </tr>
    <tr>
        <td valign=top>type</td>
        <td valign=top>e</td>
        <td valign=top>r</td>
        <td valign=top>The type describes, how the age is calculated. The following values are acceptable:
            <ul>
                <li><b>YEAR:</b> The age is calculated using the year of the meet and the year of birth only.
                <li><b>DATE:</b> The age is calculated exactly between the date and the birth date.
                <li><b>POR:</b> Age calculation according the Portuguese federation.
                <li><b>CAN.FNQ:</b> Calculation according the Quebec federation.
                <li><b>LUX:</b> Calculation according the Luxembourg federation.
            </ul>
        </td>
    </tr>
    <tr>
        <td valign=top>value</td>
        <td valign=top>d</td>
        <td valign=top>f</td>
        <td valign=top>The date value.</td>
    </tr>
</table>

### Element `<AGEGROUP />`
This element contains information about an age range. It is used in events and record lists.
<table width=100%>
    <tr>
        <th width=25%>Element/Attribute</th>
        <th width=5%>Type</th>
        <th width=5%></th>
        <th width=65%>Remarks</th>
    </tr>
    <tr>
        <td valign=top>agegroupid</td>
        <td valign=top>n</td>
        <td valign=top>r</td>
        <td valign=top>Only for events, every AGEGROUP element needs an id, because the objects can be referenced from ENTRY objects. The id has to be unique within an AGEGROUPS collection.</td>
    </tr>
    <tr>
        <td valign=top>agemax</td>
        <td valign=top>n</td>
        <td valign=top>r</td>
        <td valign=top>The upper bound of the age range. -1 means no upper bound.</td>
    </tr>
    <tr>
        <td valign=top>agemin</td>
        <td valign=top>n</td>
        <td valign=top>r</td>
        <td valign=top>The lower bound of the age range. -1 means no lower bound.</td>
    </tr>
    <tr>
        <td valign=top>gender</td>
        <td valign=top>e</td>
        <td valign=top>-</td>
        <td valign=top>In mixed events, the gender can be specifiedin the AGEGROUP objects. Values can be:
        <ul>
         <li><b>M:</b> Male
         <li><b>F:</b> Female
         <li><b>X:</b> Mixed (Is only used for relays.)
         <li><b>A:</b> All This can be useful to define events with gender set to all (A), but the ranking is separated. This attribute is not allowed in the context of a RECORDLIST or TIMESTANDARDLIST element.
        </ul></td>
    </tr>
    <tr>
        <td valign=top>calculate</td>
        <td valign=top>e</td>
        <td valign=top>-</td>
        <td valign=top>Information for relay events about how the age is calculated:
        <ul>
        <li><b>SINGLE:</b> This is the default value. The age of each relay swimmer has to be in the given range.
        <li><b>TOTAL:</b> The total age of all swimmers has to be in the given range.
        </ul></td>
    </tr>
    <tr>
        <td valign=top>handicap</td>
        <td valign=top>e</td>
        <td valign=top>-</td>
        <td valign=top>The handicap class for the agegroup. This is used to group results by disability categories. Allowed values are:
        <ul>
        <li> <b>1 - 15, 20, 34, 49</b> standard handicap classes.
        </ul></td>
    </tr>
    <tr>
        <td valign=top>levelmax</td>
        <td valign=top>s</td>
        <td valign=top>-</td>
        <td valign=top>The maximum level (A-Z) of the agegroup. If the value is missing, this means no upper bound.</td>
    </tr>
    <tr>
        <td valign=top>levelmin</td>
        <td valign=top>s</td>
        <td valign=top>-</td>
        <td valign=top>The minimum level (A-Z) of the agegroup. If the value is missing, this means no lower bound.</td>
    </tr>
    <tr>
        <td valign=top>levels</td>
        <td valign=top>s</td>
        <td valign=top>-</td>
        <td valign=top>A comma separated list of codes of allowed athlete levels.</td>
    </tr>
    <tr>
        <td valign=top>name</td>
        <td valign=top>s</td>
        <td valign=top>-</td>
        <td valign=top>The name of the age group (e.g. "Juniors").</td>
    </tr>
    <tr>
        <td valign=top>RANKINGS</td>
        <td valign=top>o</td>
        <td valign=top>-</td>
        <td valign=top>A collection with references to results ranked in this agegroup.</td>
    </tr>
</table>

### Collection `<AGEGROUPS />`
This collection contains all age group definitions of one event.

### Element `<ATHLETE />`
This contains all information of a athlete including all entries and results in the context of a meet sub tree.
<table width=100%>
    <tr>
        <th width=25%>Element/Attribute</th>
        <th width=5%>Type</th>
        <th width=5%></th>
        <th width=65%>Remarks</th>
    </tr>
    <tr>
        <td valign=top>athleteid</td>
        <td valign=top>n</td>
        <td valign=top>r</td>
        <td valign=top>The id attribute should be unique over all athletes of a meet. It is required for ATHLETE objects in a meet sub tree.</td>
    </tr>
    <tr>
        <td valign=top>birthdate</td>
        <td valign=top>d</td>
        <td valign=top>r</td>
        <td valign=top>The date of birth for the athlete. If only the year of birth is known, the date should be set to January 1st of that year.</td>
    </tr>
    <tr>
        <td valign=top>CLUB</td>
        <td valign=top>o</td>
        <td valign=top>-</td>
        <td valign=top>The club or team for the athlete, when he swam the record. This element is allowed in a RECORDLIST sub tree only.</td>
    </tr>
    <tr>
        <td valign=top>ENTRIES</td>
        <td valign=top>o</td>
        <td valign=top>-</td>
        <td valign=top>All entries of the athlete. This element is allowed in a meet sub tree only.</td>
    </tr>
    <tr>
        <td valign=top>firstname</td>
        <td valign=top>s</td>
        <td valign=top>r</td>
        <td valign=top>The first name of the athlete.</td>
    </tr>
    <tr>
        <td valign=top>firstname.en</td>
        <td valign=top>si</td>
        <td valign=top>-</td>
        <td valign=top>The first name in english.</td>
    </tr>
    <tr>
        <td valign=top>gender</td>
        <td valign=top>e</td>
        <td valign=top>r</td>
        <td valign=top>Gender of the athlete. Values can be:
        <ul>
        <li><b>M:</b> Male
        <li><b>F:</b> Female
        </ul></td>
    </tr>
    <tr>
        <td valign=top>HANDICAP</td>
        <td valign=top>o</td>
        <td valign=top>-</td>
        <td valign=top>Information about the handicap classes of a swimmer.</td>
    </tr>
    <tr>
        <td valign=top>lastname</td>
        <td valign=top>s</td>
        <td valign=top>r</td>
        <td valign=top>The last name of the athlete.</td>
    </tr>
    <tr>
        <td valign=top>lastname.en</td>
        <td valign=top>si</td>
        <td valign=top>-</td>
        <td valign=top>The last name in english.</td>
    </tr>
    <tr>
        <td valign=top>level</td>
        <td valign=top>s</td>
        <td valign=top>-</td>
        <td valign=top>The level of the athlete (used with levels in AGEGROUP).</td>
    </tr>
    <tr>
        <td valign=top>license</td>
        <td valign=top>s</td>
        <td valign=top>-</td>
        <td valign=top>The registration number given by the national federation. This number should be looked at together with the nation of the club the athlete is listed in the Lenex file.</td>
    </tr>
    <tr>
        <td valign=top>license_ipc</td>
        <td valign=top>n</td>
        <td valign=top>-</td>
        <td valign=top>The registration number given by World Para Swimming, also known as SDMS ID.</td>
    </tr>
    <tr>
        <td valign=top>nameprefix</td>
        <td valign=top>s</td>
        <td valign=top>-</td>
        <td valign=top>An optional name prefix. For example for Peter van den Hoogenband, this could be "van den".</td>
    </tr>
    <tr>
        <td valign=top>nation</td>
        <td valign=top>e</td>
        <td valign=top>-</td>
        <td valign=top>See table "Nation Codes" for acceptable values.</td>
    </tr>
    <tr>
        <td valign=top>passport</td>
        <td valign=top>s</td>
        <td valign=top>-</td>
        <td valign=top>The passport number of the athlete.</td>
    </tr>
    <tr>
        <td valign=top>RESULTS</td>
        <td valign=top>o</td>
        <td valign=top>-</td>
        <td valign=top>All results of the athlete. This element is allowed in a meet sub tree only.</td>
    </tr>
    <tr>
        <td valign=top>status</td>
        <td valign=top>e</td>
        <td valign=top>-</td>
        <td valign=top>The following values are acceptable:
        <ul>
        <li><b>EXHIBITION:</b> The athlete swims exhibition in all events.
        <li><b>FOREIGNER:</b> The athlete is a foreigner.
        <li><b>ROOKIE:</b>
        </ul>
        </td>
    </tr>
    <tr>
        <td valign=top>swrid</td>
        <td valign=top>n</td>
        <td valign=top>-</td>
        <td valign=top>The global unique athlete id given by swimrankings.net.</td>
    </tr>
</table>

### Collection `<ATHLETES />`
This collection contains all athletes of one club.

### Element `<BANK />`
This is used to represent bank payment information for a meet.
<table>
    <tr>
        <th width=25%>Element/Attribute</th>
        <th width=5%>Type</th>
        <th width=5%></th>
        <th width=65%>Remarks</th>
    </tr>
    <tr>
        <td valign=top>accountholder</td>
        <td valign=top>s</td>
        <td valign=top>-</td>
        <td valign=top>The name of the bank account holder.</td>
    </tr>
    <tr>
        <td valign=top>bic</td>
        <td valign=top>s</td>
        <td valign=top>-</td>
        <td valign=top>THE BIC code of the bank.</td>
    </tr>
    <tr>
        <td valign=top>iban</td>
        <td valign=top>s</td>
        <td valign=top>r</td>
        <td valign=top>The IBAN number of the bank account. Must be a valid IBAN number.</td>
    </tr>
    <tr>
        <td valign=top>name</td>
        <td valign=top>s</td>
        <td valign=top>-</td>
        <td valign=top>The name of the bank.</td>
    </tr>
    <tr>
        <td valign=top>note</td>
        <td valign=top>s</td>
        <td valign=top>-</td>
        <td valign=top>Note for the payment as additional information.</td>
    </tr>
</table>

### Element `<CLUB />`
In the meet sub tree, this element contains information about a club, including athletes and relays with their entries and/or results. In the record list sub tree, the element contains information about the club or nation of record holders.
<table>
    <tr>
        <th width=25%>Element/Attribute</th>
        <th width=5%>Type</th>
        <th width=5%></th>
        <th width=65%>Remarks</th>
    </tr>
    <tr>
        <td valign=top>ATHLETES <a href="#fn:1" rel="footnote">(1)</a></td>
        <td valign=top>o</td>
        <td valign=top>-</td>
        <td valign=top>The athletes of this club.</td>
    </tr>
    <tr>
        <td valign=top>code</td>
        <td valign=top>s</td>
        <td valign=top>-</td>
        <td valign=top>The official club code given by the national federation. Only official club codes should be used here!</td>
    </tr>
    <tr>
        <td valign=top>CONTACT <a href="#fn:1" rel="footnote">(1)</a></td>
        <td valign=top>o</td>
        <td valign=top>-</td>
        <td valign=top>Contact address for the specific meet.</td>
    </tr>
    <tr>
        <td valign=top>name</td>
        <td valign=top>s</td>
        <td valign=top>r</td>
        <td valign=top>The full name of the club or the team.</td>
    </tr>
    <tr>
        <td valign=top>name.en</td>
        <td valign=top>si</td>
        <td valign=top>-</td>
        <td valign=top>The club name in english.</td>
    </tr>
    <tr>
        <td valign=top>nation</td>
        <td valign=top>e</td>
        <td valign=top>-</td>
        <td valign=top>See table "Nation Codes" for acceptable values.</td>
    </tr>
    <tr>
        <td valign=top>number <a href="#fn:1" rel="footnote">(1)</a></td>
        <td valign=top>n</td>
        <td valign=top>-</td>
        <td valign=top>This value can be used to distinguish different teams of the same club in a meet entries or results file.</td>
    </tr>
    <tr>
        <td valign=top>OFFICIALS <a href="#fn:1" rel="footnote">(1)</a></td>
        <td valign=top>o</td>
        <td valign=top>-</td>
        <td valign=top>The officials from this club.</td>
    </tr>
    <tr>
        <td valign=top>region</td>
        <td valign=top>s</td>
        <td valign=top>-</td>
        <td valign=top>The code of the regional or local swimming committee. Only official codes should be used here!</td>
    </tr>
    <tr>
        <td valign=top>RELAYS <a href="#fn:1" rel="footnote">(1)</a></td>
        <td valign=top>o</td>
        <td valign=top>-</td>
        <td valign=top>The relay teams of this club.</td>
    </tr>
    <tr>
        <td valign=top>shortname</td>
        <td valign=top>s</td>
        <td valign=top>-</td>
        <td valign=top>A short version of the club name. This string is limited to 20 characters.</td>
    </tr>
    <tr>
        <td valign=top>shortname.en</td>
        <td valign=top>si</td>
        <td valign=top>-</td>
        <td valign=top>The short version of the club name in english.</td>
    </tr>
    <tr>
        <td valign=top>swrid</td>
        <td valign=top>n</td>
        <td valign=top>-</td>
        <td valign=top>The global unique club id given by swimrankings.net.</td>
    </tr>
    <tr>
        <td valign=top>type</td>
        <td valign=top>e</td>
        <td valign=top>-</td>
        <td valign=top>The following types of clubs are allowed:
        <ul>
        <li><b>CLUB:</b> This is the default value.
        <li><b>NATIONALTEAM::</b> The club represents a national team of a federation. In this case, the code, region and nation attribute should be the same.
        <li><b>REGIONALTEAM:</b> The club represents a regional team. In this case, the code and region attribute should be the same.
        <li><b>UNATTACHED:</b> To be used for the CLUB entry, that contains data of athletes, where the club is unknown. In this case, the attribute name and the CONTACT element are not required.
        </ul>
        </td>
    </tr>
</table>
<ol>
<li id="fn:1">
<p>These objects and elements are not used in CLUB objects, which appear in the RECORDLIST sub tree.</p></li>
</ol>

### Collection `<CLUBS />`
This collection contains all clubs that take part of one meet.

### Element `<CONSTRUCTOR />`
This element contains information about the software, which created the Lenex file and the contact information about the provider of that software.
<table>
    <tr>
        <th width=25%>Element/Attribute</th>
        <th width=5%>Type</th>
        <th width=5%></th>
        <th width=65%>Remarks</th>
    </tr>
    <tr>
        <td valign=top>CONTACT</td>
        <td valign=top>o</td>
        <td valign=top>r</td>
        <td valign=top>Contact information of the provider of the software, which created the Lenex file.</td>
    </tr>
    <tr>
        <td valign=top>name</td>
        <td valign=top>s</td>
        <td valign=top>-</td>
        <td valign=top>Name of the application that created the Lenex file.</td>
    </tr>
    <tr>
        <td valign=top>registration</td>
        <td valign=top>s</td>
        <td valign=top>r</td>
        <td valign=top>Name of user, to who the creator application was registered.</td>
    </tr>
    <tr>
        <td valign=top>version</td>
        <td valign=top>s</td>
        <td valign=top>r</td>
        <td valign=top>The version number of the application that created the Lenex file.</td>
    </tr>
</table>

### Element `<CONTACT />`
This element contains the contact address for a person or organisation.
<table>
    <tr>
        <th width=25%>Element/Attribute</th>
        <th width=5%>Type</th>
        <th width=5%></th>
        <th width=65%>Remarks</th>
    </tr>
    <tr>
        <td valign=top>city</td>
        <td valign=top>s</td>
        <td valign=top>-</td>
        <td valign=top>The city of the contact address.</td>
    </tr>
    <tr>
        <td valign=top>country</td>
        <td valign=top>s</td>
        <td valign=top>-</td>
        <td valign=top>See table "Country codes" for acceptable values.</td>
    </tr>
    <tr>
        <td valign=top>email</td>
        <td valign=top>s</td>
        <td valign=top>r</td>
        <td valign=top>The e-mail address of the contact. The attribute is required in the context of a CONSTRUCTOR element only.</td>
    </tr>
    <tr>
        <td valign=top>fax</td>
        <td valign=top>s</td>
        <td valign=top>-</td>
        <td valign=top>The fax number of the contact.</td>
    </tr>
    <tr>
        <td valign=top>internet</td>
        <td valign=top>s</td>
        <td valign=top>-</td>
        <td valign=top>The full URL of the website of the contact person or organisation. The https:// should be included in the string.</td>
    </tr>
    <tr>
        <td valign=top>name <a href="#fn:2" rel="footnote">(1)</a></td>
        <td valign=top>s</td>
        <td valign=top>-</td>
        <td valign=top>The full name of the contact person or the name of the organisation.</td>
    </tr>
    <tr>
        <td valign=top>mobile</td>
        <td valign=top>s</td>
        <td valign=top>-</td>
        <td valign=top>The mobile phone number of the contact person.</td>
    </tr>
    <tr>
        <td valign=top>phone</td>
        <td valign=top>s</td>
        <td valign=top>-</td>
        <td valign=top>The phone number of the contact person or the organisation.</td>
    </tr>
    <tr>
        <td valign=top>state</td>
        <td valign=top>s</td>
        <td valign=top>-</td>
        <td valign=top>The state, province or county of the contact address.</td>
    </tr>
    <tr>
        <td valign=top>street</td>
        <td valign=top>s</td>
        <td valign=top>-</td>
        <td valign=top>The first additional line of the address.</td>
    </tr>
    <tr>
        <td valign=top>street2</td>
        <td valign=top>s</td>
        <td valign=top>-</td>
        <td valign=top>The second additional line of the address.</td>
    </tr>
    <tr>
        <td valign=top>zip</td>
        <td valign=top>s</td>
        <td valign=top>-</td>
        <td valign=top>The postal code of the address.</td>
    </tr>
</table>
<ol>
<li id="fn:2">
<p>These elements are not used in the context of an OFFICIAL element.</p></li>
</ol>

### Collection `<ENTRIES />`
This collection contains all entries of on athlete or a relay team.

### Element `<ENTRY />`
This element contains the information for a single entry of an athlete or a relay to a specific round of a meet.
<table>
    <tr>
        <th width=25%>Element/Attribute</th>
        <th width=5%>Type</th>
        <th width=5%></th>
        <th width=65%>Remarks</th>
    </tr>
    <tr>
        <td valign=top>agegroupid</td>
        <td valign=top>n</td>
        <td valign=top>-</td>
        <td valign=top>Reference to an age group (AGEGROUP element in the AGEGROUPS collection of the EVENT element).</td>
    </tr>
    <tr>
        <td valign=top>entrycourse</td>
        <td valign=top>e</td>
        <td valign=top>-</td>
        <td valign=top>This attribute indicates a pool length for the entry time. This is necessary when special seeding rules are used. See section 5.4. for acceptable values.</td>
    </tr>
    <tr>
        <td valign=top>entrydistance</td>
        <td valign=top>n</td>
        <td valign=top>-</td>
        <td valign=top>The entry distance in centimeters. Is used for some fin swimming events. For such entries the entrytime should be "NT".</td>
    </tr>
    <tr>
        <td valign=top>entrytime</td>
        <td valign=top>st</td>
        <td valign=top>-</td>
        <td valign=top>The entry time in the swim time format.</td>
    </tr>
    <tr>
        <td valign=top>eventid</td>
        <td valign=top>n</td>
        <td valign=top>r</td>
        <td valign=top>Reference to the EVENT element using the id attribute.</td>
    </tr>
    <tr>
        <td valign=top>handicap</td>
        <td valign=top>e</td>
        <td valign=top>-</td>
        <td valign=top>In special cases, the sport class can be different for a single entry. Allowed values are the values for standard sport classes. <sup>(we recommend not using this)</sup>
    </td>
    </tr>
    <tr>
        <td valign=top>heatid</td>
        <td valign=top>n</td>
        <td valign=top>-</td>
        <td valign=top>Reference to a heat (HEAT element in HEATS collection of the EVENT element).</td>
    </tr>
    <tr>
        <td valign=top>lane</td>
        <td valign=top>n</td>
        <td valign=top>-</td>
        <td valign=top>The lane number of the entry.</td>
    </tr>
    <tr>
        <td valign=top>MEETINFO</td>
        <td valign=top>o</td>
        <td valign=top>-</td>
        <td valign=top>This element contains the information, about a qualification result for the entry time was achieved.</td>
    </tr>
    <tr>
        <td valign=top>RELAYPOSITIONS</td>
        <td valign=top>o</td>
        <td valign=top>-</td>
        <td valign=top>Only for relay entries. This element contains references to the relay swimmers.</td>
    </tr>
    <tr>
        <td valign=top>status</td>
        <td valign=top>e</td>
        <td valign=top>-</td>
        <td valign=top>This attribute is used for the entry status information. An empty status attribute means a regular entry. The following values are allowed:
        <ul>
        <li><b>EXH:</b> exhibition swim.
        <li><b>RJC:</b> rejected entry.
        <li><b>SICK:</b> athlete is sick.
        <li><b>WDR:</b> athlete/relay was withdrawn.
        </ul>
        </td>
    </tr>
</table>
The combination of the attributes eventid, heatid and lane should be unique over all ENTRY objects of the same meet.

### Element `<EVENT />`
This element contains all information of an event. For events with finals, there has to be an EVENT element for each round.
<table>
    <tr>
        <th width=25%>Element/Attribute</th>
        <th width=5%>Type</th>
        <th width=5%></th>
        <th width=65%>Remarks</th>
    </tr>
    <tr>
        <td valign=top>AGEGROUPS</td>
        <td valign=top>o</td>
        <td valign=top>-</td>
        <td valign=top>The AGEGROUPS collection contains the descriptions for the age groups in this event. For Open/Senior events, AGEGROUPS is only needed with one AGEGROUP element as a placeholder for the RANKINGS element (for places in result lists). If round="FHT", then no AGEGROUPS element is allowed.</td>
    </tr>
    <tr>
        <td valign=top>daytime</td>
        <td valign=top>t</td>
        <td valign=top>-</td>
        <td valign=top>The daytime of the start of the event.</td>
    </tr>
    <tr>
        <td valign=top>eventid</td>
        <td valign=top>n</td>
        <td valign=top>r</td>
        <td valign=top>Every event needs to have an id attribute, so that it can be referenced by ENTRY and RESULT objects. The id attribute has to be unique over all EVENT objects of all sessions of a meet.</td>
    </tr>
    <tr>
        <td valign=top>FEE</td>
        <td valign=top>o</td>
        <td valign=top>-</td>
        <td valign=top>The entry fee for this event. If there are global fees per athlete, relay and/or meet, the FEE elements in the MEET element should be used.</td>
    </tr>
    <tr>
        <td valign=top>gender</td>
        <td valign=top>e</td>
        <td valign=top>-</td>
        <td valign=top>The gender of the event. The following values are allowed:
        <ul>
        <li><b>A:</b> The default value is all.
        <li><b>M:</b> male.
        <li><b>F:</b> female.
        <li><b>X:</b> mixed. Mixed is only used for relays.
        </ul>
    </tr>
    <tr>
        <td valign=top>HEATS</td>
        <td valign=top>o</td>
        <td valign=top>-</td>
        <td valign=top>Collection with all heats in the event.</td>
    </tr>
    <tr>
        <td valign=top>maxentries</td>
        <td valign=top>n</td>
        <td valign=top>-</td>
        <td valign=top>The maximum number of entries per club in this event. To limit the number of entries per athlete or relay, use the maxentries attribute in the MEET element.</td>
    </tr>
    <tr>
        <td valign=top>number</td>
        <td valign=top>n</td>
        <td valign=top>r</td>
        <td valign=top>The number of the event. The event numbers should be unique over all events of a meet. The EVENT objects of the different rounds for the same event may have the same event number.</td>
    </tr>
    <tr>
        <td valign=top>order</td>
        <td valign=top>n</td>
        <td valign=top>-</td>
        <td valign=top>This value can be used to define the order of the events within a session if it is not by the event number and if there are no start times for the events.</td>
    </tr>
    <tr>
        <td valign=top>preveventid</td>
        <td valign=top>n</td>
        <td valign=top>-</td>
        <td valign=top>This value is a reference to a previous event's id. (e.g. the prelims events for final events). The default value is -1 and means, that there was no previous event.</td>
    </tr>
    <tr>
        <td valign=top>round</td>
        <td valign=top>e</td>
        <td valign=top>-</td>
        <td valign=top>The following values are allowed here:
        <ul>
        <li><b>TIM:</b> This is the default value. Used for an event with timed finals.
        <li><b>FHT:</b> Fastest heats of an event with timed finals. Events with this value for round should always refer to the corresponding timed final event, which should be of the same distance, stroke and age groups. Events with round set to FHT only make sence for the schedule and ENTRY objects, but never to be used for RESULT's.
        <li><b>FIN:</b> This is used for finals including A, B, C, … finals.
        <li><b>SEM:</b> for semi finals.
        <li><b>QUA:</b> for quarterfinals.
        <li><b>PRE:</b> for prelims.
        <li><b>SOP:</b> Swim-Off after prelims.
        <li><b>SOS:</b> Swim-Off after semi-finals.
        <li><b>SOQ:</b>Swim-Off after quarterfinals.
        <li><b>TIMETRIAL:</b> Time trial event.
        </ul>
        </td>
    </tr>
    <tr>
        <td valign=top>run</td>
        <td valign=top>n</td>
        <td valign=top>-</td>
        <td valign=top>Used if there is more than one swim-off necessary. Default value is 1.</td>
    </tr>
    <tr>
        <td valign=top>SWIMSTYLE</td>
        <td valign=top>o</td>
        <td valign=top>r</td>
        <td valign=top>The SWIMSTYLE element contains information about distance and stroke of the event.</td>
    </tr>
    <tr>
        <td valign=top>TIMESTANDARDREFS</td>
        <td valign=top>o</td>
        <td valign=top>-</td>
        <td valign=top>A list of references to TIMESTANDARDREF elements with references to time standard lists to be used for this event.</td>
    </tr>
    <tr>
        <td valign=top>timing</td>
        <td valign=top>e</td>
        <td valign=top>-</td>
        <td valign=top>The type of timing for an event. If missing, the session should be checked and finally the value for the meet should be used. See MEET for acceptable values.</td>
    </tr>
    <tr>
        <td valign=top>type</td>
        <td valign=top>e</td>
        <td valign=top>-</td>
        <td valign=top>The following values are allowed:
        <ul>
        <li>The default value is empty. This applies for regular events that are run according to the FINA (AQUA) rules.
        <li><b>MASTERS:</b> Master results to be used for master rankings/records.
        </ul>
        </td>
    </tr>
</table>

### Collection `<EVENTS />`
This collection contains all events of one session.

### Element `<FACILITY />`
This element contains name and full address of meets facility (pool).
<table>
    <tr>
        <th width=25%>Element/Attribute</th>
        <th width=5%>Type</th>
        <th width=5%></th>
        <th width=65%>Remarks</th>
    </tr>
    <tr>
        <td valign=top>city</td>
        <td valign=top>s</td>
        <td valign=top>r</td>
        <td valign=top>The city of the facility.</td>
    </tr>
    <tr>
        <td valign=top>nation</td>
        <td valign=top>s</td>
        <td valign=top>r</td>
        <td valign=top>See table "Country codes" for acceptable values.</td>
    </tr>
    <tr>
        <td valign=top>name</td>
        <td valign=top>s</td>
        <td valign=top>-</td>
        <td valign=top>The name of the facility (e.g. "Aquatic Center").</td>
    </tr>
    <tr>
        <td valign=top>state</td>
        <td valign=top>s</td>
        <td valign=top>-</td>
        <td valign=top>The state, province or county of the address.</td>
    </tr>
    <tr>
        <td valign=top>street</td>
        <td valign=top>s</td>
        <td valign=top>-</td>
        <td valign=top>The first additional line of the address.</td>
    </tr>
    <tr>
        <td valign=top>street2</td>
        <td valign=top>s</td>
        <td valign=top>-</td>
        <td valign=top>The second additional line of the address.</td>
    </tr>
    <tr>
        <td valign=top>zip</td>
        <td valign=top>s</td>
        <td valign=top>-</td>
        <td valign=top>The postal code of the address.</td>
    </tr>
</table>

### Element `<FEE />`
The fee is used in MEET and EVENT objects.
<table>
    <tr>
        <th width=25%>Element/Attribute</th>
        <th width=5%>Type</th>
        <th width=5%></th>
        <th width=65%>Remarks</th>
    </tr>
    <tr>
        <td valign=top>currency</td>
        <td valign=top>e</td>
        <td valign=top>-</td>
        <td valign=top>See table "Currency Codes" for acceptable values.</td>
    </tr>
    <tr>
        <td valign=top>type <a href="#fn:3" rel="footnote">(1)</a></td>
        <td valign=top>e</td>
        <td valign=top>r</td>
        <td valign=top>Used in context of FEES in MEET or SESSION objects only. Acceptable values are:
        <ul>
        <li><b>CLUB:</b> global fee to be paid per club for the meet.
        <li><b>ATHLETE:</b> global fee to be paid per athlete.
        <li><b>RELAY:</b> global fee to be paid per relay team.
        <li><b>TEAM:</b> global fee to be paid per team (e.g. Swiss Team Championship)
        <li><b>LATEENTRY.INDIVIDUAL:</b> fee per entry for individual late entries. This value is used in the context of MEET.FEES only.
        <li><b>LATEENTRY.RELAY:</b> fee per entry for relay late entries. This value is used in the context of MEET.FEES only.
        </ul>
        </td>
    </tr>
    <tr>
        <td valign=top>value</td>
        <td valign=top>c</td>
        <td valign=top>r</td>
        <td valign=top>The value of the fee in the currency format.</td>
    </tr>
</table>
<ol>
<li id="fn:3">
<p>This element is required only in the context of a FEES collection.</p></li>
</ol>

### Collection `<FEES />`
This collection contains all global fees in a MEET element.

### Element `<HANDICAP />`
The handicap is used for handicapped athletes.
<table>
    <tr>
        <th width=25%>Element/Attribute</th>
        <th width=5%>Type</th>
        <th width=5%></th>
        <th width=65%>Remarks</th>
    </tr>
    <tr>
        <td valign=top>breast</td>
        <td valign=top>e</td>
        <td valign=top>r</td>
        <td valign=top>The handicap class for breaststroke. Allowed values are:
        <ul>
        <li><b>0 - 15:</b> standard handicap classes. The number is equal to the sport class (e.g. SB5 - 5)
        </ul>
        </td>
    </tr>
    <tr>
        <td valign=top>breaststatus</td>
        <td valign=top>e</td>
        <td valign=top>-</td>
        <td valign=top>The state of the sport class for breaststroke. <a href="https://github.com/SwimStandardHub/lenex/issues/9">(#9)</a> Allowed values are:
        <ul>
        <li><b>NONE:</b> The sport class have no official confirmation. (default value) <a href="https://github.com/SwimStandardHub/lenex/issues/15">(#15)</a>
        <li><b>NATIONAL:</b> The sport class is on national level only.
        <li><b>NEW:</b> The sport class is not yet valid.
        <li><b>REVIEW:</b> The sport class must be reviewed in this year, but is valid up to the end of the year.
        <li><b>OBSERVATION:</b> : The sport class need the observation during the meet in order to get the confirmed status.
        <li><b>CONFIRMED:</b> The sport class is confirmed for international meets.
        </ul>
        </td>
    </tr>
    <tr>
        <td valign=top>exception</td>
        <td valign=top>s</td>
        <td valign=top>-</td>
        <td valign=top>The codes of exceptions according to the WPS rules.</td>
    </tr>
    <tr>
        <td valign=top>free</td>
        <td valign=top>e</td>
        <td valign=top>r</td>
        <td valign=top>The handicap class for freestyle, backstroke and fly. Allowed values are:
        <ul>
        <li><b>0 - 15:</b> standard handicap classes. The number is equal to the sport class (e.g. S5 - 5)
        </ul>
    </tr>
    <tr>
        <td valign=top>freestatus</td>
        <td valign=top>e</td>
        <td valign=top>-</td>
        <td valign=top>The state of the sport class for freestyle, backstroke and fly. Same values used as for breaststatus. <a href="https://github.com/SwimStandardHub/lenex/issues/9">(#9)</a>
        </td>
    </tr>
    <tr>
        <td valign=top>medley</td>
        <td valign=top>e</td>
        <td valign=top>r</td>
        <td valign=top>The handicap class for individual medley. Allowed values are:
        <ul>
        <li><b>0 - 15:</b> standard handicap classes. The number is equal to the sport class (e.g. SM5 - 5)
        </ul>
    </tr>
    <tr>
        <td valign=top>medleystatus</td>
        <td valign=top>e</td>
        <td valign=top>-</td>
        <td valign=top>The state of the sport class for individual medley. Same values used as for breaststatus. <a href="https://github.com/SwimStandardHub/lenex/issues/9">(#9)</a>
        </td>
    </tr>
</table>

### Element `<HEAT />`
The heat is used to define more details in the start list (e.g. schedule).
<table>
    <tr>
        <th width=25%>Element/Attribute</th>
        <th width=5%>Type</th>
        <th width=5%></th>
        <th width=65%>Remarks</th>
    </tr>
    <tr>
        <td valign=top>agegroupid</td>
        <td valign=top>n</td>
        <td valign=top>-</td>
        <td valign=top>Reference to an age group (AGEGROUP element in the AGEGROUPS collection of the EVENT element).</td>
    </tr>
    <tr>
        <td valign=top>daytime</td>
        <td valign=top>t</td>
        <td valign=top>-</td>
        <td valign=top>The daytime of the start of the event.</td>
    </tr>
    <tr>
        <td valign=top>final</td>
        <td valign=top>e</td>
        <td valign=top>-</td>
        <td valign=top>This value is used to identify A, B, ... finals. Allowed are characters A, B, C and D.</td>
    </tr>
    <tr>
        <td valign=top>heatid</td>
        <td valign=top>n</td>
        <td valign=top>r</td>
        <td valign=top>The id attribute should be unique over all heats of a meet. It is required when you have ENTRY / RESULT objects that reference to a heat.</td>
    </tr>s
    <tr>
        <td valign=top>number</td>
        <td valign=top>n</td>
        <td valign=top>r</td>
        <td valign=top>The number of the heat. The heat numbers have to be unique within one event, also in a case when you have A finals in different agegroups.</td>
    </tr>
    <tr>
        <td valign=top>order</td>
        <td valign=top>n</td>
        <td valign=top>-</td>
        <td valign=top>This value can be used to define the order of the heats within an event if it is not by the heat number and if there are no start times for the heats.</td>
    </tr>
    <tr>
        <td valign=top>status</td>
        <td valign=top>e</td>
        <td valign=top>-</td>
        <td valign=top>The status of the heat. Allowed values are:
        <ul>
        <li><b>SCHEDULED:</b>  The heat is scheduled but not seeded yet. <a href="https://github.com/SwimStandardHub/lenex/issues/8">(#8)</a>
        <li><b>SEEDED:</b> The heat is seeded.
        <li><b>INOFFICIAL:</b> Results are available but not official.
        <li><b>OFFICIAL:</b> Results of the heat are official.
        </ul>
        </td>
    </tr>
</table>

### Collection `<HEATS />`
This collection contains all heats of one event.

### Element `<JUDGE />`
This element contains information to attach an official to a session with his role in the session.
<table>
    <tr>
        <th width=25%>Element/Attribute</th>
        <th width=5%>Type</th>
        <th width=5%></th>
        <th width=65%>Remarks</th>
    </tr>
    <tr>
        <td valign=top>number</td>
        <td valign=top>n</td>
        <td valign=top>-</td>
        <td valign=top>The number for judges where there are more than one. This can be used for example for the lane number for timekeepers.</td>
    </tr>
    <tr>
        <td valign=top>officialid</td>
        <td valign=top>n</td>
        <td valign=top>r</td>
        <td valign=top>A reference to a OFFICIAL element.</td>
    </tr>
    <tr>
        <td valign=top>remarks</td>
        <td valign=top>s</td>
        <td valign=top>-</td>
        <td valign=top>Additional information for the judge.</td>
    </tr>
    <tr>
        <td valign=top>role</td>
        <td valign=top>e</td>
        <td valign=top>-</td>
        <td valign=top>Indicates the role of a judge. The list is built according to the FINA descriptions. Acceptable values are:
        <ul>
        <li><b>OTH:</b> other or unknown. This is the default value.
        <li><b>MDR:</b> The meet director.
        <li><b>TDG:</b> The technical delegate.
        <li><b>REF:</b> The referees.
        <li><b>STA:</b> The starters.
        <li><b>ANN:</b> The announcers or speakers.
        <li><b>JOS:</b> The judge of strokes.
        <li><b>CTIK:</b> The chief timekeeper.
        <li><b>TIK:</b> The timekeepers.
        <li><b>CFIN:</b> The chief finish judge.
        <li><b>FIN:</b> The finish judges.
        <li><b>CIOT:</b> The chief inspectors of turns.
        <li><b>IOT:</b> The inspectors of turns.
        <li><b>FSR:</b> The false start rope personnel.
        <li><b>COC:</b> The clerks of course.
        <li><b>CREC:</b> The chief recorders.
        <li><b>REC:</b> The recorders.
        <li><b>CRS:</b> The control room supervisor.
        <li><b>CR:</b> Control room (computerroom).
        <li><b>MED:</b> Medical service.
        </ul>
        </td>
    </tr>
</table>

### Collection `<JUDGES />`
This collection contains all judges of one session.

### Element `<LENEX />`
This is the root element of every Lenex file which identifies it as a XML file conforming to the Lenex data format.
<table>
    <tr>
        <th width=25%>Element/Attribute</th>
        <th width=5%>Type</th>
        <th width=5%></th>
        <th width=65%>Remarks</th>
    </tr>
    <tr>
        <td valign=top>CONSTRUCTOR</td>
        <td valign=top>o</td>
        <td valign=top>r</td>
        <td valign=top>This element contains information about the software which created the Lenex file.</td>
    </tr>
    <tr>
        <td valign=top>MEETS</td>
        <td valign=top>o</td>
        <td valign=top>-</td>
        <td valign=top>Contains all the information of meets like athletes, relays, entries and results.</td>
    </tr>
    <tr>
        <td valign=top>RECORDLISTS</td>
        <td valign=top>o</td>
        <td valign=top>-</td>
        <td valign=top>Contains different types of records (e.g. World records, Olympic records) including age group records.</td>
    </tr>
    <tr>
        <td valign=top>TIMESTANDARDLISTS</td>
        <td valign=top>o</td>
        <td valign=top>-</td>
        <td valign=top>Contains different type of time standards and qualification times.</td>
    </tr>
    <tr>
        <td valign=top>revision</td>
        <td valign=top>s</td>
        <td valign=top>-</td>
        <td valign=top>The patch or revision version number of the Lenex format. If this value is missing, the initial version from the attribut <code>version</code> is referenced. <a href="https://github.com/SwimStandardHub/lenex/issues/15">(#15)</a></td>
    </tr>
    <tr>
        <td valign=top>version</td>
        <td valign=top>s</td>
        <td valign=top>r</td>
        <td valign=top>The version number of the Lenex format. The value for this document version is <code>3.1</code><a href="https://github.com/SwimStandardHub/lenex/issues/15">(#15)</a></td>
    </tr>     
</table>

### Element `<MEET />`
This element contains all information of one meet, including events, athletes, relays, entries and results.
<table>
    <tr>
        <th width=25%>Element/Attribute</th>
        <th width=5%>Type</th>
        <th width=5%></th>
        <th width=65%>Remarks</th>
    </tr>
    <tr>
        <td valign=top>AGEDATE</td>
        <td valign=top>o</td>
        <td valign=top>-</td>
        <td valign=top>The date to be used to calculate the age of athletes. The default value is the date of the first session and type by year of birth only.</td>
    </tr>
    <tr>
        <td valign=top>BANK</td>
        <td valign=top>o</td>
        <td valign=top>-</td>
        <td valign=top>Information for a bank account for payment of entry fees.</td>
    </tr>
    <tr>
        <td valign=top>altitude</td>
        <td valign=top>n</td>
        <td valign=top>-</td>
        <td valign=top>Height above sea level of the meet city.</td>
    </tr>
    <tr>
        <td valign=top>city</td>
        <td valign=top>s</td>
        <td valign=top>r</td>
        <td valign=top>The name of the city where the meet was run. Should be the same as FACILITY.city</td>
    </tr>
    <tr>
        <td valign=top>city.en</td>
        <td valign=top>si</td>
        <td valign=top>-</td>
        <td valign=top>Name of meet city in english.</td>
    </tr>
    <tr>
        <td valign=top>CLUBS</td>
        <td valign=top>o</td>
        <td valign=top>-</td>
        <td valign=top>Collection of clubs of the meet.</td>
    </tr>
    <tr>
        <td valign=top>CONTACT</td>
        <td valign=top>o</td>
        <td valign=top>-</td>
        <td valign=top>The contact address of the meet organizer.</td>
    </tr>
    <tr>
        <td valign=top>course</td>
        <td valign=top>e</td>
        <td valign=top>-</td>
        <td valign=top>The size of the pool. See section 5.4. for acceptable values. If the attribute is not available, all SESSION objects need to have a course attribute.</td>
    </tr>
    <tr>
        <td valign=top>deadline</td>
        <td valign=top>d</td>
        <td valign=top>-</td>
        <td valign=top>The date for the entry deadline.</td>
    </tr>
    <tr>
        <td valign=top>deadlinetime</td>
        <td valign=top>t</td>
        <td valign=top>-</td>
        <td valign=top>The exact time for the entry deadline.</td>
    </tr>
    <tr>
        <td valign=top>entrystartdate</td>
        <td valign=top>d</td>
        <td valign=top>-</td>
        <td valign=top>The date from when (online) entries are open/available.</td>
    </tr>
    <tr>
        <td valign=top>entrytype</td>
        <td valign=top>e</td>
        <td valign=top>-</td>
        <td valign=top>The type of (online) entries:
        <ul>
        <li><b>OPEN:</b> The meet is open for all clubs.
        <li><b>INVITATION:</b> The meet is open to invited clubs only.
        </ul>
        </td>
    </tr>
    <tr>
        <td valign=top>FACILITY</td>
        <td valign=top>o</td>
        <td valign=top>-</td>
        <td valign=top>The full address of the meets facility.</td>
    </tr>
    <tr>
        <td valign=top>FEES</td>
        <td valign=top>o</td>
        <td valign=top>-</td>
        <td valign=top>Fees used for this meet. On this level, different global fees for clubs, athletes and relays are allowed. If there are fees that have to be paid per entry, the FEE element in the EVENT objects should be used.</td>
    </tr>
    <tr>
        <td valign=top>hostclub</td>
        <td valign=top>s</td>
        <td valign=top>-</td>
        <td valign=top>The executing federation or club of the meet (e.g. the German Swimming Federation, if the European Champ was held in Berlin).</td>
    </tr>
    <tr>
        <td valign=top>hostclub.url</td>
        <td valign=top>s</td>
        <td valign=top>-</td>
        <td valign=top>A website url, that directs to the executing club for the meet.</td>
    </tr>
    <tr>
        <td valign=top>maxentriesathlete</td>
        <td valign=top>n</td>
        <td valign=top>-</td>
        <td valign=top>The maximum number of individual entries per athlete in this meet.</td>
    </tr>
    <tr>
        <td valign=top>maxentriesrelay</td>
        <td valign=top>n</td>
        <td valign=top>-</td>
        <td valign=top>The maximum number of relay entries per club in this meet.</td>
    </tr>
    <tr>
        <td valign=top>name</td>
        <td valign=top>s</td>
        <td valign=top>r</td>
        <td valign=top>The name of the meet. Normally the name should not contain a full date (maybe the year only) and/or a city or pool name.</td>
    </tr>
    <tr>
        <td valign=top>name.en</td>
        <td valign=top>si</td>
        <td valign=top>-</td>
        <td valign=top>Meet name in english.</td>
    </tr>
    <tr>
        <td valign=top>nation</td>
        <td valign=top>e</td>
        <td valign=top>r</td>
        <td valign=top>The three letter code of the nation of the meet city. This should be the same as FACILITY.nation</td>
    </tr>
    <tr>
        <td valign=top>number</td>
        <td valign=top>s</td>
        <td valign=top>-</td>
        <td valign=top>The sanction number for the meet by the federation.</td>
    </tr>
    <tr>
        <td valign=top>organizer</td>
        <td valign=top>s</td>
        <td valign=top>-</td>
        <td valign=top>The organisation which promotes the meet (e.g. AQUA for the World Championships).</td>
    </tr>
    <tr>
        <td valign=top>organizer.url</td>
        <td valign=top>s</td>
        <td valign=top>-</td>
        <td valign=top>A website url, that directs to the organizer of the meet.</td>
    </tr>
    <tr>
        <td valign=top>POINTTABLE</td>
        <td valign=top>o</td>
        <td valign=top>-</td>
        <td valign=top>Description of the point table used for scoring.</td>
    </tr>
    <tr>
        <td valign=top>POOL</td>
        <td valign=top>o</td>
        <td valign=top>-</td>
        <td valign=top>Details about the pool where the meet took place.</td>
    </tr>
    <tr>
        <td valign=top>QUALIFY</td>
        <td valign=top>o</td>
        <td valign=top>-</td>
        <td valign=top>Details about how qualification times for entries are defined.</td>
    </tr>
    <tr>
        <td valign=top>reservecount</td>
        <td valign=top>n</td>
        <td valign=top>-</td>
        <td valign=top>The number of reserve swimmers in finals and semifinals.</td>
    </tr>
    <tr>
        <td valign=top>result.url</td>
        <td valign=top>s</td>
        <td valign=top>-</td>
        <td valign=top>A website url, that directs to the results page. This should be a deep (direct) link to the result lists and not the general website of a meet.</td>
    </tr>
    <tr>
        <td valign=top>SESSIONS</td>
        <td valign=top>o</td>
        <td valign=top>r</td>
        <td valign=top>Description of all events grouped by session.</td>
    </tr>
    <tr>
        <td valign=top>startmethod</td>
        <td valign=top>e</td>
        <td valign=top>-</td>
        <td valign=top>The following values are possible: <a href="https://github.com/SwimStandardHub/lenex/issues/6">(#6)</a>
        <ul>
        <li><b>1:</b> one start allowed (default value)
        <li><b>2:</b> tow starts allowed
        </ul>
        </td>
    </tr>
    <tr>
        <td valign=top>swrid</td>
        <td valign=top>uid</td>
        <td valign=top>-</td>
        <td valign=top>The global unique meet id given by swimrankings.net.</td>
    </tr>
    <tr>
        <td valign=top>timing</td>
        <td valign=top>e</td>
        <td valign=top>-</td>
        <td valign=top>The type of timing for a meet. Acceptable values are:
        <ul>
        <li><b>AUTOMATIC:</b> A full automatic timing system was used.
        <li><b>SEMIAUTOMATIC:</b> Automatic start, manual button for end time.
        <li><b>MANUAL3:</b> Timing was done with three manual times per lane.
        <li><b>MANUAL2:</b> Timing was done with two manual times per lane.
        <li><b>MANUAL1:</b> Timing was done with one manual time per lane.
        </ul>
        </td>
    </tr>
    <tr>
        <td valign=top>touchpadmode</td>
        <td valign=top>e</td>
        <td valign=top>-</td>
        <td valign=top>Information about timing installation for a meet. Acceptable values are:
        <ul>
        <li><b>ONESIDE:</b> Touchpads on one side of the pool.
        <li><b>BOTHSIDE:</b> Touchpads on both sides of the pool.
        </ul>
        </td>
    </tr>
    <tr>
        <td valign=top>type</td>
        <td valign=top>e</td>
        <td valign=top>-</td>
        <td valign=top>The meet type. The following values are allowed:
        <ul>
        <li>The default value is empty. This applies for normal meets that are run according to the FINA rules.
        <li>All other values depend on definitions of national federations.
        </ul>
        </td>
    </tr>
    <tr>
        <td valign=top>withdrawuntil</td>
        <td valign=top>d</td>
        <td valign=top>-</td>
        <td valign=top>The date for withdrawals from the entry list.</td>
    </tr>
</table>

### Element `<MEETINFO />`
This element is used in entries and records for general information about a meet.
<table>
    <tr>
        <th width=25%>Element/Attribute</th>
        <th width=5%>Type</th>
        <th width=5%></th>
        <th width=65%>Remarks</th>
    </tr>
    <tr>
        <td valign=top>approved <a href="#fn:5" rel="footnote">(2)</a></td>
        <td valign=top>s</td>
        <td valign=top>-</td>
        <td valign=top>Contains a code for the organisation, who approved the qualification time, e.g. AQUA, LEN or a IOC nation code. If this field is empty, the qualification time was not approved.</td>
    </tr>
    <tr>
        <td valign=top>city <a href="#fn:4" rel="footnote">(1)</a></td>
        <td valign=top>s</td>
        <td valign=top>r</td>
        <td valign=top>The city name where the meet took place.</td>
    </tr>
    <tr>
        <td valign=top>course <a href="#fn:5" rel="footnote">(2)</a></td>
        <td valign=top>e</td>
        <td valign=top>-</td>
        <td valign=top>This attribute indicates the pool length, where the qualification time was achieved. See section 5.4. for acceptable values.</td>
    </tr>
    <tr>
        <td valign=top>date <a href="#fn:4" rel="footnote">(1)</a></td>
        <td valign=top>d</td>
        <td valign=top>r</td>
        <td valign=top>The date of the swim of the record or qualification time achievement.</td>
    </tr>
    <tr>
        <td valign=top>daytime</td>
        <td valign=top>t</td>
        <td valign=top>-</td>
        <td valign=top>The day time of the swim.</td>
    </tr>
    <tr>
        <td valign=top>name</td>
        <td valign=top>s</td>
        <td valign=top>-</td>
        <td valign=top>The meet name.</td>
    </tr>
    <tr>
        <td valign=top>nation <a href="#fn:4" rel="footnote">(1)</a></td>
        <td valign=top>e</td>
        <td valign=top>r</td>
        <td valign=top>The nation of the city for the meet.</td>
    </tr>
    <tr>
        <td valign=top>POOL</td>
        <td valign=top>o</td>
        <td valign=top>-</td>
        <td valign=top>The details about the pool.</td>
    </tr>
    <tr>
        <td valign=top>qualificationtime <a href="#fn:5" rel="footnote">(2)</a></td>
        <td valign=top>st</td>
        <td valign=top>-</td>
        <td valign=top>The qualification time, since this can be different to the entry time. If the value is missing, the entry time is the qualification time.</td>
    </tr>
    <tr>
        <td valign=top>state</td>
        <td valign=top>s</td>
        <td valign=top>-</td>
        <td valign=top>The state of the city for the meet.</td>
    </tr>
    <tr>
        <td valign=top>timing</td>
        <td valign=top>e</td>
        <td valign=top>-</td>
        <td valign=top>The type of timing. See MEET for acceptable values.</td>
    </tr>
</table>
<ol>
<li id="fn:4">
<p>These elements are required only in the context of a RECORD element.</p></li>
<li id="fn:5">
<p>These elements are used in the context of a ENTRY / RELAYPOSITION element only.</p></li>
</ol>

### Collection `<MEETS />`
This collection allows you to put the results of more than one meet in the same Lenex file. However, our experience with Lenex during the last years shows that it is better to keep different meets in separate files.

### Element `<OFFICIAL />`
This element contains all information about an official.
<table>
    <tr>
        <th width=25%>Element/Attribute</th>
        <th width=5%>Type</th>
        <th width=5%></th>
        <th width=65%>Remarks</th>
    </tr>
    <tr>
        <td valign=top>CONTACT</td>
        <td valign=top>o</td>
        <td valign=top>-</td>
        <td valign=top>The contact information of the official.</td>
    </tr>
    <tr>
        <td valign=top>firstname</td>
        <td valign=top>s</td>
        <td valign=top>r</td>
        <td valign=top>The first name if the official.</td>
    </tr>
    <tr>
        <td valign=top>gender</td>
        <td valign=top>e</td>
        <td valign=top>-</td>
        <td valign=top>Gender of the official. Values can be male (M) and female (F).</td>
    </tr>
    <tr>
        <td valign=top>grade</td>
        <td valign=top>s</td>
        <td valign=top>-</td>
        <td valign=top>The grade of the judge. The value here is specific to national federations and depends on their officials education system.</td>
    </tr>
    <tr>
        <td valign=top>lastname</td>
        <td valign=top>s</td>
        <td valign=top>r</td>
        <td valign=top>The last name of the official.</td>
    </tr>
    <tr>
        <td valign=top>license</td>
        <td valign=top>s</td>
        <td valign=top>-</td>
        <td valign=top>The registration number given by the national federation.</td>
    </tr>
    <tr>
        <td valign=top>nameprefix</td>
        <td valign=top>s</td>
        <td valign=top>-</td>
        <td valign=top>An optional name prefix. For example for Peter van den Hoogenband, this could be "van den".</td>
    </tr>
    <tr>
        <td valign=top>nation</td>
        <td valign=top>e</td>
        <td valign=top>-</td>
        <td valign=top>See table "Nation Codes" for acceptable values.</td>
    </tr>
    <tr>
        <td valign=top>officialid</td>
        <td valign=top>n</td>
        <td valign=top>r</td>
        <td valign=top>The id attribute should be unique over all officials of a meet. It is required for JUDGE objects in a meet sub tree.</td>
    </tr>
    <tr>
        <td valign=top>passport</td>
        <td valign=top>s</td>
        <td valign=top>-</td>
        <td valign=top>The passport number of the official.</td>
    </tr>
</table>

### Collection `<OFFICIALS />`
This collection contains all officials of a club.

### Element `<POINTTABLE />`
This element is used to describe the point scoring used for a meet.
<table>
    <tr>
        <th width=25%>Element/Attribute</th>
        <th width=5%>Type</th>
        <th width=5%></th>
        <th width=65%>Remarks</th>
    </tr>
    <tr>
        <td valign=top>name</td>
        <td valign=top>s</td>
        <td valign=top>r</td>
        <td valign=top>The name of the point score system.</td>
    </tr>
    <tr>
        <td valign=top>pointtableid</td>
        <td valign=top>e</td>
        <td valign=top>-</td>
        <td valign=top>Common point tables have a unique id. Details are in chapter 5.5.</td>
    </tr>
    <tr>
        <td valign=top>version</td>
        <td valign=top>s</td>
        <td valign=top>r</td>
        <td valign=top>The version number/year of the point score system.</td>
    </tr>
</table>

### Element `<POOL />`
This element is used to describe the pool where the meet took place.
<table>
    <tr>
        <th width=25%>Element/Attribute</th>
        <th width=5%>Type</th>
        <th width=5%></th>
        <th width=65%>Remarks</th>
    </tr>
    <tr>
        <td valign=top>lanemax</td>
        <td valign=top>n</td>
        <td valign=top>-</td>
        <td valign=top>Number of the last lane used in the pool for the meet. The number of lanes can be calculated with LANEMAX - LANEMIN + 1.</td>
    </tr>
    <tr>
        <td valign=top>lanemin</td>
        <td valign=top>n</td>
        <td valign=top>-</td>
        <td valign=top>Number of the first lane used in the pool for the meet.</td>
    </tr>
    <tr>
        <td valign=top>temperature</td>
        <td valign=top>n</td>
        <td valign=top>-</td>
        <td valign=top>The water temperature.</td>
    </tr>
    <tr>
        <td valign=top>type</td>
        <td valign=top>e</td>
        <td valign=top>-</td>
        <td valign=top>The type of the pool. Acceptable values are:
        <ul>
        <li><b>INDOOR</b>
        <li><b>OUTDOOR</b>
        <li><b>LAKE</b>
        <li><b>OCEAN</b>
        </ul>
        </td>
    </tr>
</table>

### Element `<QUALIFY />`
This element contains information about details, how qualification entrytimes are defined.
<table>
    <tr>
        <th width=25%>Element/Attribute</th>
        <th width=5%>Type</th>
        <th width=5%></th>
        <th width=65%>Remarks</th>
    </tr>
    <tr>
        <td valign=top>conversion</td>
        <td valign=top>e</td>
        <td valign=top>-</td>
        <td valign=top>The way, how times are converted for seeding.
        <ul>
        <li><b>NONE:</b> This is the default value. No conversion will be done.
        <li><b>FINA_POINTS:</b> Entry times, flaged with a course other than the event course will be converted by calculating the FINA points and from there calculating back to a swim time.
        <li><b>PERCENT_LINEAR:</b> Here the conversion will be done by adding / subtracting a certain percantage to the original entry time.
        <li><b>NON_CONFORMING_LAST:</b> In this case, entries that are flaged with the event course will be seeded first, all other entries will be seeded after these.</td>
        </ul>
        </td>
    </tr>
    <tr>
        <td valign=top>from</td>
        <td valign=top>d</td>
        <td valign=top>r</td>
        <td valign=top>The first day of the qualification period for entry times.</td>
    </tr>
    <tr>
        <td valign=top>percent</td>
        <td valign=top>n</td>
        <td valign=top>-</td>
        <td valign=top>The percentage for conversion PERCENT_LINEAR.</td>
    </tr>
    <tr>
        <td valign=top>until</td>
        <td valign=top>d</td>
        <td valign=top>-</td>
        <td valign=top>The last day of the qualification period for entry times. If it is missing, then the default is the last day before the first day of the meet.</td>
    </tr>
</table>

### Element `<RANKING />`
This element describes one entry in the rankings of one agegroup. It contains the place and a reference to a result (individual or relay).
<table>
    <tr>
        <th width=25%>Element/Attribute</th>
        <th width=5%>Type</th>
        <th width=5%></th>
        <th width=65%>Remarks</th>
    </tr>
    <tr>
        <td valign=top>order</td>
        <td valign=top>n</td>
        <td valign=top>-</td>
        <td valign=top>This value can be used to define the order of the results. If it is missing, the value for place is used to sort the elements in a collection.</td>
    </tr>
    <tr>
        <td valign=top>place</td>
        <td valign=top>n</td>
        <td valign=top>r</td>
        <td valign=top>The final position in the result list for the current event/round.</td>
    </tr>
    <tr>
        <td valign=top>resultid</td>
        <td valign=top>n</td>
        <td valign=top>r</td>
        <td valign=top>A reference to the RESULT element.</td>
    </tr>
</table>

### Collection `<RANKINGS />`
This collection contains a set of ranking elements.

### Element `<RECORD />`
This element describes one individual or relay record. It is possible to have no ATHLETE / RELAY objects. In this case the record is a "record standard time".
<table>
    <tr>
        <th width=25%>Element/Attribute</th>
        <th width=5%>Type</th>
        <th width=5%></th>
        <th width=65%>Remarks</th>
    </tr>
    <tr>
        <td valign=top>ATHLETE</td>
        <td valign=top>o</td>
        <td valign=top>-</td>
        <td valign=top>The person who holds the record. This is only used for individual records.</td>
    </tr>
    <tr>
        <td valign=top>comment</td>
        <td valign=top>s</td>
        <td valign=top>-</td>
        <td valign=top>This value can be used for additional comments like "Swum in the prelims" or things like that.</td>
    </tr>
    <tr>
        <td valign=top>MEETINFO</td>
        <td valign=top>o</td>
        <td valign=top>-</td>
        <td valign=top>Information about the meet, where the record was swum.</td>
    </tr>
    <tr>
        <td valign=top>RELAY</td>
        <td valign=top>o</td>
        <td valign=top>-</td>
        <td valign=top>The relay team and swimmers, who holds the record. This is only used for relay records.</td>
    </tr>
    <tr>
        <td valign=top>SPLITS</td>
        <td valign=top>o</td>
        <td valign=top>-</td>
        <td valign=top>The split times of the record.</td>
    </tr>
    <tr>
        <td valign=top>SWIMSTYLE</td>
        <td valign=top>o</td>
        <td valign=top>r</td>
        <td valign=top>The swimstyle contains information like distance, stroke of the record.</td>
    </tr>
    <tr>
        <td valign=top>swimtime</td>
        <td valign=top>st</td>
        <td valign=top>r</td>
        <td valign=top>The final time of the record in the swim time format.</td>
    </tr>
    <tr>
        <td valign=top>status</td>
        <td valign=top>e</td>
        <td valign=top>-</td>
        <td valign=top>State of the record. The following values are allowed:
        <ul>
        <li><b>APPROVED:</b> Record is approved and valid. (default value)
        <li><b>PENDING:</b> Waiting for ratification by the corresponding authority.
        <li><b><s>INVALID:</s></b> Invalid due ratification failures. <sup>(we recommend not using this)</sup>
        <li><b><s>APPROVED.HISTORY:</s></b> Valid, but not current record anymore. <sup>(we recommend not using this)</sup>
        <li><b><s>PENDING.HISTORY:</s></b> Waiting, but not current record anymore. <sup>(we recommend not using this)</sup>
        <li><b>TARGETTIME:</b> No record yet but a time to define the minimal time for a new record.
        </ul>
        <a href="https://github.com/SwimStandardHub/lenex/discussions/13">discussion about the values</a>
        </td>
    </tr>
</table>

### Element `<RECORDLIST />`
This element describes one single record list.
<table>
    <tr>
        <th width=25%>Element/Attribute</th>
        <th width=5%>Type</th>
        <th width=5%></th>
        <th width=65%>Remarks</th>
    </tr>
    <tr>
        <td valign=top>AGEGROUP</td>
        <td valign=top>o</td>
        <td valign=top>-</td>
        <td valign=top>For agegroup records. Agegroup is "Open", if the element is missing.</td>
    </tr>
    <tr>
        <td valign=top>course</td>
        <td valign=top>e</td>
        <td valign=top>r</td>
        <td valign=top>The course for the record list. See section 5.4. for acceptable values.</td>
    </tr>
    <tr>
        <td valign=top>gender</td>
        <td valign=top>e</td>
        <td valign=top>r</td>
        <td valign=top>The gender for records in this list. Acceptable values are:
        <ul>
        <li><b>M:</b> male.
        <li><b>F:</b> female.
        <li><b>X:</b> mixed. Mixed is only used for relays.
        </ul>
        </td>
    </tr>
    <tr>
        <td valign=top>handicap</td>
        <td valign=top>e</td>
        <td valign=top>-</td>
        <td valign=top>The handicap class for the record list. Allowed values are:
        <ul>
        <li>1 - 15, 20, 34, 49 standard handicap classes.
        </ul>
        </td>
    </tr>
    <tr>
        <td valign=top>name</td>
        <td valign=top>s</td>
        <td valign=top>r</td>
        <td valign=top>The name of the record list (e.g. "World Records").</td>
    </tr>
    <tr>
        <td valign=top>nation</td>
        <td valign=top>s</td>
        <td valign=top>-</td>
        <td valign=top>For international records, this field is empty. For national or regional records, the field should contain the FINA three letter code of the national federation.</td>
    </tr>
    <tr>
        <td valign=top>order</td>
        <td valign=top>n</td>
        <td valign=top>-</td>
        <td valign=top>This value can be used to define an order for all recordlists within a collection.</td>
    </tr>
    <tr>
        <td valign=top>RECORDS</td>
        <td valign=top>o</td>
        <td valign=top>r</td>
        <td valign=top>The records of this record list.</td>
    </tr>
    <tr>
        <td valign=top>region</td>
        <td valign=top>s</td>
        <td valign=top>-</td>
        <td valign=top>For international and national records, this field is empty. For regional records, the field should contain the official code for the region. If region has a value, nation needs to have a value as well.</td>
    </tr>
    <tr>
        <td valign=top>updated</td>
        <td valign=top>d</td>
        <td valign=top>-</td>
        <td valign=top>The date of the last change to the record list.</td>
    </tr>
    <tr>
        <td valign=top>type</td>
        <td valign=top>e</td>
        <td valign=top>-</td>
        <td valign=top>The record type. The following values are allowed:
        <ul>
        <li><b>WR</b> World records.
        <li><b>OR</b> Olympic records.
        <li><b>ER</b> European records.
        <li><b>PAR</b> Pan American records.
        <li><b>AFR</b> African records.
        <li><b>AR</b> Asian records.
        <li><b>OCR</b> Oceanian records.
        <li><b>CWR</b> Commonwealth records.
        <li>FINA three letter nation code: The national records of the specific federation.
        </ul>
        This list of types maybe extended by federations. In this case, values should have the nation code and a dot as prefix (e.g. <b>SUI.RZW</b> for records of a region in Switzerland).
        </td>
    </tr>
</table>

### Collection `<RECORDLISTS />`
This collection contains a set of record lists. For each different combination of gender, course, age group or type, a separate RECORDLIST element is needed.

### Collection `<RECORDS />`
This collection contains all records of a record list. If there is more than one athlete holding the same record, each of them has a RECORD element in the collection.

### Element `<RELAY />`
This element is used to describe one relay team for a record or for a meet.
<table>
    <tr>
        <th width=25%>Element/Attribute</th>
        <th width=5%>Type</th>
        <th width=5%></th>
        <th width=65%>Remarks</th>
    </tr>
    <tr>
        <td valign=top>agemax <a href="#fn:7" rel="footnote">(2)</a></td>
        <td valign=top>n</td>
        <td valign=top>r</td>
        <td valign=top>The maximum age allowed for the oldest swimmer in the team. The value -1 means no upper bound.</td>
    </tr>
    <tr>
        <td valign=top>agemin <a href="#fn:7" rel="footnote">(2)</a></td>
        <td valign=top>n</td>
        <td valign=top>r</td>
        <td valign=top>The minimal age allowed for the youngest swimmer in the team. The value -1 means no lower bound.</td>
    </tr>
    <tr>
        <td valign=top>agetotalmax <a href="#fn:7" rel="footnote">(2)</a></td>
        <td valign=top>n</td>
        <td valign=top>r</td>
        <td valign=top>The maximum total age of all swimmers in the relay team. The value -1 means that the total age is unknown.</td>
    </tr>
    <tr>
        <td valign=top>agetotalmin <a href="#fn:7" rel="footnote">(2)</a></td>
        <td valign=top>n</td>
        <td valign=top>r</td>
        <td valign=top>The minimum total age of all swimmers in the relay team. The value -1 means that the total age is unknown.</td>
    </tr>
    <tr>
        <td valign=top>CLUB <a href="#fn:6" rel="footnote">(1)</a></td>
        <td valign=top>o</td>
        <td valign=top>-</td>
        <td valign=top>The club or team of the relay in the context of a record.</td>
    </tr>
    <tr>
        <td valign=top>ENTRIES <a href="#fn:7" rel="footnote">(2)</a></td>
        <td valign=top>o</td>
        <td valign=top>-</td>
        <td valign=top>All entries of the relay team.</td>
    </tr>
    <tr>
        <td valign=top>gender <a href="#fn:7" rel="footnote">(2)</a></td>
        <td valign=top>e</td>
        <td valign=top>r</td>
        <td valign=top>The gender of the relay team. Acceptable values are:
        <ul>
        <li><b>M:</b> male.
        <li><b>F:</b> female.
        <li><b>X:</b> mixed.
        </ul>
        </td>
    </tr>
    <tr>
        <td valign=top>handicap <a href="#fn:7" rel="footnote">(2)</a></td>
        <td valign=top>e</td>
        <td valign=top>-</td>
        <td valign=top>For relays with handicapped swimmers. The default value is 0. Other values allowed are:
        <ul>
        <li><b>14:</b> relay with athletes with a mental handicap. For II (S14) athletes only.
        <li><b>20:</b> relay athletes of a total of 0-20 handicap points. For PI (S1-S10) athletes only.
        <li><b>34:</b> relay athletes of a total of 21-34 handicap points. For PI (S1-S10) athletes only.
        <li><b>49:</b> relay athletes of a total of 35 handicap points or more. For VI (S11-S13) athletes only.
        </ul>
        </td>
    </tr>
    <tr>
        <td valign=top>name</td>
        <td valign=top>s</td>
        <td valign=top>-</td>
        <td valign=top>The name of the relay team.</td>
    </tr>
    <tr>
        <td valign=top>number <a href="#fn:7" rel="footnote">(2)</a></td>
        <td valign=top>n</td>
        <td valign=top>-</td>
        <td valign=top>The team number of the relay team. This number is only necessary, if there are different teams of the same club in the same agegroups / events.</td>
    </tr>
    <tr>
        <td valign=top>RELAYPOSITIONS <a href="#fn:6" rel="footnote">(1)</a></td>
        <td valign=top>o</td>
        <td valign=top>-</td>
        <td valign=top>The relay swimmers in the context of a relay record.</td>
    </tr>
    <tr>
        <td valign=top>RESULTS <a href="#fn:7" rel="footnote">(2)</a></td>
        <td valign=top>o</td>
        <td valign=top>-</td>
        <td valign=top>All results of the relay team.</td>
    </tr>
</table>
<ol>
<li id="fn:6">
<p>These objects are allowed in the context of a record only.</p></li>
<li id="fn:7">
<p>These elements/objects are allowed in the context of a meet only.</p></li>
</ol>

### Collection `<RELAYS />`
This collection contains all relays of one club of a meet.

### Element `<RELAYPOSITION />`
This element is used for information about one relay swimmer.
<table>
    <tr>
        <th width=25%>Element/Attribute</th>
        <th width=5%>Type</th>
        <th width=5%></th>
        <th width=65%>Remarks</th>
    </tr>
    <tr>
        <td valign=top>ATHLETE</td>
        <td valign=top>o</td>
        <td valign=top><-/td>
        <td valign=top>Last name, first name, etc. of the athlete. This element is allowed in the context of a record only and in this case it is required.</td>
    </tr>
    <tr>
        <td valign=top>athleteid</td>
        <td valign=top>n</td>
        <td valign=top>-</td>
        <td valign=top>A reference to the ATHLETE element of the athlete. This attribute is allowed in the context of a meet sub tree only.</td>
    </tr>
    <tr>
        <td valign=top>MEETINFO</td>
        <td valign=top>o</td>
        <td valign=top>-</td>
        <td valign=top>This element contains the information, where the entry time was achieved. This element is only allowed in the context of a relay entry.</td>
    </tr>
    <tr>
        <td valign=top>number</td>
        <td valign=top>n</td>
        <td valign=top>r</td>
        <td valign=top>The number of the swimmer in the relay. The first swimmer is 1, the second 2 and so on. -1 can be used to add reserve swimmers.</td>
    </tr>
    <tr>
        <td valign=top>reactiontime</td>
        <td valign=top>rt</td>
        <td valign=top>-</td>
        <td valign=top>The reaction time at the start of the first swimmer and the relay take over times for other swimmers.</td>
    </tr>
    <tr>
        <td valign=top>status</td>
        <td valign=top>e</td>
        <td valign=top>-</td>
        <td valign=top>No status attribute means the swimmer finished his part correctly. Otherwise, the following values are allowed:
        <ul>
        <li><b>DSQ:</b> relay athlete was disqualified.
        <li><b>DNF:</b> relay athlete did not finish.
        </ul>
        </td>
    </tr>
</table>

### Collection `<RELAYPOSITIONS />`
This collection contains information's about relay swimmers.

### Element `<RESULT />`
This element is used to describe one result of a swimmer or relay team.
<table>
    <tr>
        <th width=25%>Element/Attribute</th>
        <th width=5%>Type</th>
        <th width=5%></th>
        <th width=65%>Remarks</th>
    </tr>
    <tr>
        <td valign=top>comment</td>
        <td valign=top>s</td>
        <td valign=top>-</td>
        <td valign=top>Additional comment e.g. for new records or reasons for disqualifications.</td>
    </tr>
    <tr>
        <td valign=top>eventid</td>
        <td valign=top>n</td>
        <td valign=top>-</td>
        <td valign=top>Reference to the EVENT element using the id attribute.</td>
    </tr>
    <tr>
        <td valign=top>handicap</td>
        <td valign=top>e</td>
        <td valign=top>-</td>
        <td valign=top>In special cases, the sport class can be different for a single result. Allowed values are the values for standard sport classes. <a href="https://github.com/SwimStandardHub/lenex/issues/5">(#5)</a>
    </td>
    </tr>
    <tr>
        <td valign=top>heatid</td>
        <td valign=top>n</td>
        <td valign=top>-</td>
        <td valign=top>Reference to a heat (HEAT element in HEATS collection of the EVENT element).</td>
    </tr>
    <tr>
        <td valign=top>lane</td>
        <td valign=top>n</td>
        <td valign=top>-</td>
        <td valign=top>The lane number of the entry.</td>
    </tr>
    <tr>
        <td valign=top>points</td>
        <td valign=top>n</td>
        <td valign=top>-</td>
        <td valign=top>The number of points for the result according to the scoring table used in a meet.</td>
    </tr>
    <tr>
        <td valign=top>reactiontime</td>
        <td valign=top>rt</td>
        <td valign=top></td>
        <td valign=top>The reaction time at the start. For relay events it is the reaction time of the first swimmer.</td>
    </tr>
    <tr>
        <td valign=top>RELAYPOSITIONS</td>
        <td valign=top>o</td>
        <td valign=top>-</td>
        <td valign=top>The information about relay swimmers in this result. Only allowed for relay RESULT objects.</td>
    </tr>
    <tr>
        <td valign=top>resultid</td>
        <td valign=top>n</td>
        <td valign=top>r</td>
        <td valign=top>Each result needs a unique id which should be unique over a meet.</td>
    </tr>
    <tr>
        <td valign=top>status</td>
        <td valign=top>e</td>
        <td valign=top>-</td>
        <td valign=top>This attribute is used for the result status information. An empty status attribute means a regular result. The following values are allowed:
        <ul>
        <li><b>EXH:</b> exhibition swim.
        <li><b>DSQ:</b> athlete/relay disqualified.
        <li><b>DNS:</b> athlete/relay did not start (no reason given or to late withdrawl).
        <li><b>DNF:</b> athlete/relay did not finish.
        <li><b>SICK:</b> athlete is sick.
        <li><b>WDR:</b> athlete/relay was withdrawn (on time).
        </ul>
        </td>
    </tr>
    <tr>
        <td valign=top>SPLITS</td>
        <td valign=top>o</td>
        <td valign=top>-</td>
        <td valign=top>The split times for the result. In a Lenex file, split times are always saved continuously.</td>
    </tr>
    <tr>
        <td valign=top>swimdistance</td>
        <td valign=top>n</td>
        <td valign=top>-</td>
        <td valign=top>The result distance in centimeters. Is used for some fin swimming events. For such results the swimtime should be "NT".</td>
    </tr>
    <tr>
        <td valign=top>swimtime</td>
        <td valign=top>st</td>
        <td valign=top>r</td>
        <td valign=top>The final time of the result in the swim time format.</td>
    </tr>
</table>

### Collection `<RESULTS />`
This collection contains all results of a athlete or relay team of a meet.

### Element `<SESSION />`
This element is used to describe one session of a meet.
<table>
    <tr>
        <th width=25%>Element/Attribute</th>
        <th width=5%>Type</th>
        <th width=5%></th>
        <th width=65%>Remarks</th>
    </tr>
    <tr>
        <td valign=top>course</td>
        <td valign=top>e</td>
        <td valign=top>-</td>
        <td valign=top>With indicating a pool length per session, the global value of the meet can be overridden, e.g. if the prelim sessions are short course and the finals are long course. See section 5.4. for acceptable values.</td>
    </tr>
    <tr>
        <td valign=top>date</td>
        <td valign=top>d</td>
        <td valign=top>r</td>
        <td valign=top>The date of the session.</td>
    </tr>
    <tr>
        <td valign=top>daytime</td>
        <td valign=top>t</td>
        <td valign=top>-</td>
        <td valign=top>The daytime when the session starts.</td>
    </tr>
    <tr>
        <td valign=top>endtime</td>
        <td valign=top>t</td>
        <td valign=top>-</td>
        <td valign=top>The time when the session ended.</td>
    </tr>
    <tr>
        <td valign=top>EVENTS</td>
        <td valign=top>o</td>
        <td valign=top>r</td>
        <td valign=top>The events of the session.</td>
    </tr>
    <tr>
        <td valign=top>FEES</td>
        <td valign=top>o</td>
        <td valign=top>-</td>
        <td valign=top>Fees used for this session. On this level, different global fees for clubs, athletes and relays are allowed. If there are fees that have to be paid per entry, the FEE element in the EVENT objects should be used.</td>
    </tr>
    <tr>
        <td valign=top>JUDGES</td>
        <td valign=top>o</td>
        <td valign=top>-</td>
        <td valign=top>The judges of the session.</td>
    </tr>
    <tr>
        <td valign=top>maxentriesathlete</td>
        <td valign=top>n</td>
        <td valign=top>-</td>
        <td valign=top>The maximum number of individual entries per athlete in this session.</td>
    </tr>
    <tr>
        <td valign=top>maxentriesrelay</td>
        <td valign=top>n</td>
        <td valign=top>-</td>
        <td valign=top>The maximum number of relay entries per club in this session.</td>
    </tr>
    <tr>
        <td valign=top>name</td>
        <td valign=top>s</td>
        <td valign=top>-</td>
        <td valign=top>Additional name for the session e.g. "Day 1 - Prelims".</td>
    </tr>
    <tr>
        <td valign=top>number</td>
        <td valign=top>n</td>
        <td valign=top>r</td>
        <td valign=top>The number of the session. Session numbers in a meet have to be unique.</td>
    </tr>
    <tr>
        <td valign=top>officialmeeting</td>
        <td valign=top>t</td>
        <td valign=top>-</td>
        <td valign=top>The daytime when the officials meeting starts.</td>
    </tr>
    <tr>
        <td valign=top>POOL</td>
        <td valign=top>o</td>
        <td valign=top>-</td>
        <td valign=top>The details about the pool, if they are different per session. Otherwise use the element in MEET.</td>
    </tr>
    <tr>
        <td valign=top>remarksjudge</td>
        <td valign=top>s</td>
        <td valign=top>-</td>
        <td valign=top>Additional remarks given by the referee.</td>
    </tr>
    <tr>
        <td valign=top>teamleadermeeting</td>
        <td valign=top>t</td>
        <td valign=top>-</td>
        <td valign=top>The daytime when the team leaders meeting starts.</td>
    </tr>
    <tr>
        <td valign=top>timing</td>
        <td valign=top>e</td>
        <td valign=top>-</td>
        <td valign=top>The type of timing for a session. If missing, the global value for the meet should be used. See MEET for acceptable values.</td>
    </tr>
    <tr>
        <td valign=top>touchpadmode</td>
        <td valign=top>e</td>
        <td valign=top>-</td>
        <td valign=top>Information about timing installation for a session. See MEET for acceptable values.</td>
    </tr>
    <tr>
        <td valign=top>warmupfrom</td>
        <td valign=top>t</td>
        <td valign=top>-</td>
        <td valign=top>The daytime when the warmup starts.</td>
    </tr>
    <tr>
        <td valign=top>warmupuntil</td>
        <td valign=top>t</td>
        <td valign=top>-</td>
        <td valign=top>The daytime when the warmup ends.</td>
    </tr>
</table>

### Collection `<SESSIONS />`
Depending on the context this collection contains all sessions of a meet or all sessions for a judge, where he is planed in.

### Element `<SPLIT />`
This element contains information about a single split time. In a Lenex file, split times are always saved continuously.
<table>
    <tr>
        <th width=25%>Element/Attribute</th>
        <th width=5%>Type</th>
        <th width=5%></th>
        <th width=65%>Remarks</th>
    </tr>
    <tr>
        <td valign=top>distance</td>
        <td valign=top>n</td>
        <td valign=top>r</td>
        <td valign=top>The distance where the split time was measured.</td>
    </tr>
    <tr>
        <td valign=top>swimtime</td>
        <td valign=top>st</td>
        <td valign=top>r</td>
        <td valign=top>The time of the result in the swim time format.</td>
    </tr>
</table>

### Collection `<SPLITS />`
This collection contains all available split times for a single result.

### Element `<SWIMSTYLE />`
This element is used to describe one swim style.
<table>
    <tr>
        <th width=25%>Element/Attribute</th>
        <th width=5%>Type</th>
        <th width=5%></th>
        <th width=65%>Remarks</th>
    </tr>
    <tr>
        <td valign=top>code</td>
        <td valign=top>s</td>
        <td valign=top>-</td>
        <td valign=top>A code (max. 6 characters) of the swim style if the stroke is unknown.</td>
    </tr>
    <tr>
        <td valign=top>distance</td>
        <td valign=top>n</td>
        <td valign=top>r</td>
        <td valign=top>The distance for the event. For relay events it is the distance for one single athlete.</td>
    </tr>
    <tr>
        <td valign=top>name</td>
        <td valign=top>s</td>
        <td valign=top>-</td>
        <td valign=top>The full descriptive name of the swim style if the stroke is unknown (e.g. "5 x 75m Breast with one Arm only").</td>
    </tr>
    <tr>
        <td valign=top>relaycount</td>
        <td valign=top>n</td>
        <td valign=top>r</td>
        <td valign=top>The number of swimmers per entry / result. Value 1 means, that it is an individual event. All other values mean, that it is a relay event.</td>
    </tr>
    <tr>
        <td valign=top>stroke</td>
        <td valign=top>e</td>
        <td valign=top>r</td>
        <td valign=top>The following values are allowed here:
        <ul>
        <li><b>APNEA:</b> for apnea (fin swimming).
        <li><b>BACK:</b> for backstroke.
        <li><b>BIFINS:</b> for bi-fins (fin swimming).
        <li><b>MIXEDFINS:</b> Special mixed relay according to CMAS rules.
        <li><b>BREAST:</b> for breaststroke.
        <li><b>DYNAMIC:</b> for dynamic (fin swimming). Result is in meter, attribute swimdistance.
        <li><b>DYNAMIC_BIFINS:</b> for dynamic with bi-fins (fin swimming). Result is in meter, attribute swimdistance.
        <li><b>DYNAMIC_NOFINS:</b> for dynamic without fins (fin swimming). Result is in meter, attribute swimdistance.
        <li><b>FLY:</b> for fly or butterfly.
        <li><b>FREE:</b> for freestyle.
        <li><b>IMMERSION:</b> for immersion (fin swimming).
        <li><b>IMRELAY:</b> for relays where each athletes swims all strokes like in an individual medley event.
        <li><b>MEDLEY:</b> for individual and relay medley. The order of stroke is according to FINA rules: Fly, back, breast, free for individual events. Back, breast, fly, free for relay events.
        <li><b>SPEED_APNEA:</b> for speed apnea (fin swimming).
        <li><b>SPEED_ENDURANCE:</b> for speed endurance (fin swimming).
        <li><b>STATIC:</b> for static (fin swimming).
        <li><b>SURFACE:</b> for surface (fin swimming).
        <li><b>UNKNOWN:</b> for all special events. In this case, the name attribute of the event is mandatory.
        </ul>
        </td>
    </tr>
    <tr>
        <td valign=top>swimstyleid</td>
        <td valign=top>n</td>
        <td valign=top>-</td>
        <td valign=top>The id attribute is important for SWIMSTYLE objects, where the stroke attribute is "UNKNOWN". In this case, the id should be a unique value to help to identify the swim style.</td>
    </tr>
    <tr>
        <td valign=top>technique</td>
        <td valign=top>e</td>
        <td valign=top>-</td>
        <td valign=top>The technique of the style. If this attribute is missing or empty, it means normal swimming. All other values are mainly used for technical events in meets for kids. The following values are allowed:
        <ul>
        <li><b>DIVE:</b> for swimming under water.
        <li><b>GLIDE:</b> for gliding only.
        <li><b>KICK:</b> for kick only (Swimming with legs, no arms used).
        <li><b>PULL:</b> for pull only (Swimming with arms, no legs used).
        <li><b>START:</b> for start only.
        <li><b>TURN:</b> for turn only.
        </ul>
        </td>
    </tr>
</table>

### Element `<TIMESTANDARD />`
This element describes one time standard.
<table>
    <tr>
        <th width=25%>Element/Attribute</th>
        <th width=5%>Type</th>
        <th width=5%></th>
        <th width=65%>Remarks</th>
    </tr>
    <tr>
        <td valign=top>SWIMSTYLE</td>
        <td valign=top>o</td>
        <td valign=top>r</td>
        <td valign=top>The style contains information like distance, stroke of the record. For each TIMESTANDARD in the same collection, the SWIMSTYLE should be unique.</td>
    </tr>
    <tr>
        <td valign=top>swimtime</td>
        <td valign=top>st</td>
        <td valign=top>r</td>
        <td valign=top>The time standard or qualification time.</td>
    </tr>
</table>

### Collection `<TIMESTANDARDS />`
This collection contains a set of time standards.

### Element `<TIMESTANDARDLIST />`
This element describes one single time standard list.
<table>
    <tr>
        <th width=25%>Element/Attribute</th>
        <th width=5%>Type</th>
        <th width=5%></th>
        <th width=65%>Remarks</th>
    </tr>
    <tr>
        <td valign=top>AGEGROUP</td>
        <td valign=top>o</td>
        <td valign=top>-</td>
        <td valign=top>For age group time standards. Agegroup is "Open", if the element is missing.</td>
    </tr>
    <tr>
        <td valign=top>course</td>
        <td valign=top>e</td>
        <td valign=top>r</td>
        <td valign=top>The course for the timestandard list. See section 5.4. for acceptable values.</td>
    </tr>
    <tr>
        <td valign=top>gender</td>
        <td valign=top>e</td>
        <td valign=top>r</td>
        <td valign=top>The gender for time standards in this list. Acceptable values are:
        <ul>
        <li><b>M:</b> male.
        <li><b>F:</b> female.
        <li><b>X:</b> mixed.
        </ul>
        </td>
    </tr>
    <tr>
        <td valign=top>handicap</td>
        <td valign=top>e</td>
        <td valign=top>-</td>
        <td valign=top>The handicap class for the timestandard list. Allowed values are:
        <ul>
        <li>1 - 15, 20, 34, 49 standard handicap classes.
        </ul>
        </td>
    </tr>
    <tr>
        <td valign=top>name</td>
        <td valign=top>s</td>
        <td valign=top>r</td>
        <td valign=top>The name of the time standard list (e.g. "Olympic A Time Standards").</td>
    </tr>
    <tr>
        <td valign=top>timestandardlistid</td>
        <td valign=top>n</td>
        <td valign=top>r</td>
        <td valign=top>The unique id of the time standard list.</td>
    </tr>
    <tr>
        <td valign=top>TIMESTANDARDS</td>
        <td valign=top>o</td>
        <td valign=top>r</td>
        <td valign=top>The time standards or qualification times of this list.</td>
    </tr>
    <tr>
        <td valign=top>type</td>
        <td valign=top>e</td>
        <td valign=top>-</td>
        <td valign=top>There can be different type of time standards. Default value is MAXIMUM. The following values are allowed:
        <ul>
        <li><b>DEFAULT:</b> The time standards describe a set of default times, that may be used in a team competition, where a result for an event of a team is missing or invalid and therefore is replaced by this default time.
        <li><b>MAXIMUM:</b> The time standards describe a maximal time for a meet. Swimmers can only compete, when they are faster than the time standards.
        <li><b>MINIMUM:</b> The time standards describe a minimal time for a meet. Swimmers can only compete, when they are slower than the time standards.
        </ul>
        </td>
    </tr>
</table>

### Collection `<TIMESTANDARDLISTS />`
This collection contains a set of time standard lists. For each different combination of gender, course, age group or type, a separate TIMESTANDARDLIST element is needed.

### Element `<TIMESTANDARDREF />`
This element describes a reference from a meet to a time standard list.
<table>
    <tr>
        <th width=25%>Element/Attribute</th>
        <th width=5%>Type</th>
        <th width=5%></th>
        <th width=65%>Remarks</th>
    </tr>
    <tr>
        <td valign=top>timestandardlistid</td>
        <td valign=top>n</td>
        <td valign=top>r</td>
        <td valign=top>The id of the time standard list element.</td>
    </tr>
    <tr>
        <td valign=top>FEE</td>
        <td valign=top>o</td>
        <td valign=top>-</td>
        <td valign=top>An optional element with a fine for missed time standards.</td>
    </tr>
    <tr>
        <td valign=top>marker</td>
        <td valign=top>s</td>
        <td valign=top>-</td>
        <td valign=top>An optional string to be used to mark the result, if the time standard was missed. Or to mark a result if a qualification time was fulfilled.</td>
    </tr>
</table>

### Collection `<TIMESTANDARDREFS />`
This collection contains a set of time standard references.

## 5. Lenex data types
In a Lenex file, the following data types are used:
<table>
    <tr>
        <td valign=top>String</td>
        <td valign=top>s</td>
        <td valign=top>A string containing any character. Special characters like < > " ' and & have to be quoted with &amp;lt; &amp;gt; &amp;quot; &amp;apos; and &amp;amp;. For line ends in multiline strings you should use &amp;#10; (encoding for the LF character).</td>
    </tr>
    <tr>
        <td valign=top>String international</td>
        <td valign=top>si</td>
        <td valign=top>Same as "String" but only characters between ASCII #32 - #127 allowed.</td>
    </tr>
    <tr>
        <td valign=top>Number</td>
        <td valign=top>n</td>
        <td valign=top>A signed 32-bit integer number. Only the characters "0" .. "9" and "-" are allowed.</td>
    </tr>
    <tr>
        <td valign=top>Enumeration</td>
        <td valign=top>e</td>
        <td valign=top>An enumeration is a set of predefined values that are allowed in the attribute of that data type.</td>
    </tr>
    <tr>
        <td valign=top>Date</td>
        <td valign=top>d</td>
        <td valign=top>Dates are always represented by a string in the form "YYYY-MM-DD". Example: "2004-03-09" means March 9, 2004</td>
    </tr>
    <tr>
        <td valign=top>Daytime</td>
        <td valign=top>t</td>
        <td valign=top>A daytime (hour and minutes) represented by a string in the form "HH:MM". Hours should be from 0 to 24, minutes from 0 to 59.</td>
    </tr>
    <tr>
        <td valign=top>Currency</td>
        <td valign=top>c</td>
        <td valign=top>An integer number. Currency values are represented in cents, e.g. one dollar in the Lenex currency format is 100.</td>
    </tr>
    <tr>
        <td valign=top>Swim time</td>
        <td valign=top>st</td>
        <td valign=top>The swim time data type is always a fixed length string of the following form: "HH:MM:SS.ss". HH: hours from 0 to 99, MM: minutes from 0 to 59, SS: seconds from 0 to 59, ss: Hundreds of a second from 0 to 99. Example: "00:14:45.86" means a time of 14:45.86. In addition the string "NT" is allowed if no time is available.</td>
    </tr>
    <tr>
        <td valign=top>Reaction time</td>
        <td valign=top>rt</td>
        <td valign=top>All reaction times are numbers and are measured in hundreds of a second. The first character indicates, if the reaction time is positive (+) or negative (-). Example "+14" means a positive reaction time of 14 hundreds. The reaction time "0.00" should be transmitted as "0". If the reactiontime is missing, the value should be empty.</td>
    </tr>
    <tr>
        <td valign=top>Unique id</td>
        <td valign=top>uid</td>
        <td valign=top>Unique id's are a character (A-Z) followed by a number. Additional separator characters (space, dash, point) are allowed but have no meaning and should be ignored when comparing unique id's. The id's are unique worldwide and are handled by swimrankings.net.</td>
    </tr>
</table>

### 5.1 Nation codes
For the nation codes, the three letter codes of FINA are used. The current table with all codes and nation names can be found in the file "Lenex_Nation.txt".

### 5.2. Country codes
For the country codes, the international two letter postal codes are used. The current table with all codes and country names can be found in the file "Lenex_Country.txt".

### 5.3. Currency codes
For the currency codes, the international three letter codes are used. The current table with all codes and currency names can be found in the file "Lenex_Currency.txt".

### 5.4. Course codes
For the course attribute, the following values are allowed:
* LCM, 
* SCM, 
* SCY, 
* SCM16, 
* SCM20, 
* SCM33, 
* SCY20, 
* SCY27, 
* SCY33, 
* SCY36 and 
* OPEN for open water swimming.

### 5.5. ID's for POINTTABLE elements
For common point scorings there is a list of id's that can / should be used for the POINTTABLE elements. It can be found in the file "Lenex_PointTable.txt".

## 6. Specific extensions for different federations

### 6.1 Germany (GER)
Element ATHLETE can have the attribute "license_dbs" for the registration id of the german para swimming federation. "license_dsv" is used for the german federation id (DSV).

For the round attribute in EVENT there is an additional value: "GER.RES". This means "Nachschwimmen" or re-swim and is used for the German team championships.

For stroke attribute in SWIMSTYLE there is an additional value "GER.APH" for "Apnoe mit Hebeboje".

### 6.2 Switzerland (SUI)
For the status attribute in ATHLETE there is an additional value: "SUI.STARTSUISSE ". This is a special registration for foreign athletes to be allow to swim in a relay for a Swiss Record.

## 7. Frequently asked questions (FAQ)

**(Q)** What should I put in the required attributes for an ATHLETE in a RECORD when there is no record but a required time only?<br>
**(A)** The ATHLETE / RELAY element is not required in a RECORD. None of both elements should be there in such a case.

## 8. Version History
|Date|change or addition|
|----|----|
|05. Dec 2024|Minor version number changed to version 3.1|
|05. Dec 2024|The enum of HANDICAP.sportclass status changed|
|22. Nov 2024|Init of this document|
