# mir DB design

## Contents

* [DB accessors](#db-accessors) 
* [DB Queries](#db-queries) 

  * [Owner queries](#owner-queries) 
    * [Owner: Profile view RV.5.0](#owner-profile-view-rv-5-0)
    * [Owner: Generate Condition Vessel Survey view RV.19.0](#owner-gen-cond-vessel-survey-rv-19-0)
    * [```DELETE {baseUri}/users/owners/{user_idx}```](#delete-users-idx)
    * [```PATCH {baseUri}/users/owners/{user_idx}```](#patch-users-idx)

## <a name="db-accessors"></a> DB accessors

Below is the list of entities that require access to the DB of the mir-system

- Owner
- Surveyor
- Data scientist
- DB Administrator

Each of these entities poses different queries to the DB. We identify the latter below.

## <a name="db-queries"></a> DB Queries

Below are the queries that each of the identified entities may pose to the
mir-system database.

### <a name="owner-queries"></a> Owner queries

Owner queries are app-level queries i.e. posed via the mir web/mobile app. The following
types of queries have been identified for an owner

- Retrieve information queries
- Create new resources queries
- Delete resources queries
- Update resource queries

#### <a name="owner-profile-view-rv-5-0"></a> Owner: Profile view RV.5.0

When a user is at their profile they view a list of vessels they have registered.
This implies that  a search query is posed on the mir DB. Also a search box for 
searching into the vessels collection is provided

The following are the queries that an owner can generate via their profile view

- Get owner vessels: GET /owners/user_idx/vessels 
- Search over the vessels an owner has created using filters GET /owners/user_idx/vessels  either of the following criteria
  - vessel name
  - mmsi
  - ce
  
Constraints:

- The user_idx should represent an owner

Both calls return a list of vessel ids registered by the owner
  
#### Owner: Surveys view RV.9.0

The owner from their profile view can click on the **Your Surveys** button and view 
a list of surveys they have generated

- Get owner surveys: GET /owners/{user_idx}/surveys

Constraints:

- The user_idx should represent an owner

This call simply returns a list of surveys ids

#### Owner: Register Vessel view RV.10.0

The owner from their profile can click on the **Register Vessel** button 
in order to be able to register a vessel. This creates a new resource

 
- Register a vessel: POST /owners/{user_idx}/vessels

Constraints:

- The user_idx should represent an owner


#### Owner: Vessel Profile view RV.7.0

From the list of vessels that the user is presented in their profile, they can click on a link and view the vessel profile. 
In this view we have the following queries

- Get basic info about vessel GET /vessels/{vessel_idx} limited with: 
   - Name
   - Length 
   - Beam
   - Propulsion type
   - Vessel surveys (sorted with the newest at the top)
   
- Operational profile GET /surveys/{survey_idx}
  - filter only vessel health related info
  
- Outstanding Items GET /surveys/{survey_idx}
  - filter only survey recommendations related info



#### Owner: Profile Settings view RV.8.0
TODO

#### Owner: Generate Survey view RV. 16.0.

The owner from the profile of a registered vessel can generate surveys. This is done
by clicking on the **Generate Survey** button which leads them to this view. In this view an
API call is made to get the survey types supported by the mir-system

- GET /surveys/types


These types are shown as a drop down menu to the user


#### <a name="owner-gen-cond-vessel-survey-rv-19-0"></a> Owner: Generate Condition Vessel Survey view RV.19.0

In this view the user is prompted to take photos of various parts 
of the vessel. The application queries which parts are appropriate for the
specific vessel type and the specific survey type

- GET /surveys/{vessel_type}/parts

The application creates a list of items out of these parts


#### Owner: Add Vessel Part Photo view RV.17.0

Once the owner clicks on a vessel part from the [Owner: Generate Condition Vessel Survey view RV.19.0](#owner-gen-cond-vessel-survey-rv-19-0)


#### 

There exist the following queries

- App-level queries
  - Owner queries
  - Surveyor queries
  
The identified owner queries are

  - Get vessel
  - Get vessels (of specific type)  
  
- Modelling level queries

  - Fetch data
  - Clean data
  - Store data
  - Pass data to model
  - Store trained model
  - Fetch trained model
  
## Data design

Given the queries above we identify the following entities

- Owners
- Surveyors
- Vessels
- Surveys

AWS S3 is used to support the mir data lake. This is a general data storage platform for unstructured data like images and documents. 
  
  
## Conventions 

### Naming conventions
