## Implementation Details

Google partial response pattern is implemented for dicussion api. All the GET requests support partial response.

To know more vist [google partial response api reference](https://developers.google.com/gdata/docs/2.0/reference#PartialResponse)


The partial response pattern also supports expand functionality. Expand means that a nested sub-object for a resource can be requested.

For Ex:

`
[GET] http://localhost:10010/discussions?fields=id,messages
`

This api call means fetch all discussions and only respond with id and messages field. There is a hasMany association mapping between discussion and messages.

The keys in the field parameter are first validated for corrosponding property in response object. Then the associations for that object is searched if a match is found for the requested field than that nested resource is eagerly loaded. If a match is not found than error will be returned to client

There are four types of associations identified. For each type, the filter is constructed from the association metadata to fetch the correct data. This process continues on the result data recursively until either no more data or no more fields parameter.

- parent-child association
- child-parent association
- self association
- reverse-self association

Since the expanding process in partial response relys on the association info, it's very important to set up the associations correctly in the model definition.

The model definition in the Sequelize module provides a detail of all associations.