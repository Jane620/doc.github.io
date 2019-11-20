UC2: Configuration file loading
=============================================


Purpose
~~~~~~~~~~~~~
Load NetworkPlan to create needed MOs for Fronthaul configuration.


Actors
~~~~~~~~~~~~~~
cOAM


Precondition
~~~~~~~~~~~~~
1. FH app startup and RCP services are running.
2. OAM-CM start and running by infrastructure.
3. REM/eBBC register LIM:ACTIVATION_REQ/CONFIGURATION_REQ.


Description
~~~~~~~~~~~~~~~~
1. Get NetworkPlan from OMCM
  1.1 FH config RestAPI POST to OMCM [EXEC 1]
    1. Response 200 OK
    2. Return 400/? , retry 1.1 1x
    3. timeout(how many times) , retry 1x
  1.2 RestAPI GET [EXEC 2]
    1. Response 200 with json format content which list all MOs
    2. Return 400/404, retry 1.2 1x (404 need retry 1.2?)
    3. timeout(how many times) , retry 1x
2. Parse the NetworkPlan
  2.1 FHS insert MOs to IMS
