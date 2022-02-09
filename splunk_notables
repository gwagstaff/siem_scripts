import requests


# https://docs.splunk.com/Documentation/ES/7.0.0/API/NotableEventAPIreference
# derived from https://gist.github.com/LukeMurphey/b45a5425572deffd2e955e460f261dbf
def updateNotableEvents(sessionKey, baseurl, comment, status=None, urgency=None, owner=None, eventIDs=None, searchID=None):
    """
    Update some notable events.
 
    Arguments:
    sessionKey -- The session key to use
    baseurl -- The URL of splunkd (e.g. "https://localhost:8089/")
    comment -- A description of the change or some information about the notable events
    status -- A status (only required if you are changing the status of the event)
    urgency -- An urgency (only required if you are changing the urgency of the event)
    owner -- A nowner (only required if reassigning the event)
    eventIDs -- A list of notable event IDs (must be provided if a search ID is not provided)
    searchID -- An ID of a search. All of the events associated with this search will be modified unless a list of eventIDs are provided that limit the scope to a sub-set of the results.
    """
 
    # Make sure that the session ID was provided
    if sessionKey is None:
        raise Exception("A session key was not provided")
 
    # Make sure that rule IDs and/or a search ID is provided
    if eventIDs is None and searchID is None:
        raise Exception("Either eventIDs of a searchID must be provided (or both)")
        return False
 
    # These the arguments to the REST handler
    args = {}
    args['comment'] = comment
 
    if status is not None:
        args['status'] = status
 
    if urgency is not None:
        args['urgency'] = urgency
 
    if owner is not None:
        args['newOwner'] = owner
 
    # Provide the list of event IDs that you want to change:
    if eventIDs is not None:
        args['ruleUIDs'] = eventIDs
 
    # If you want to manipulate the notable events returned by a search then include the search ID
    if searchID is not None:
        args['searchID'] = searchID


    auth_header = {'Authorization': 'Splunk %s' % sessionKey}

    args['output_mode'] = 'json'
    mod_notables = requests.post(baseurl + 'services/notable_update', data=args, headers=auth_header, verify=False)
    
    return mod_notables.json()
