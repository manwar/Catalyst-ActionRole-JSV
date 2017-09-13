# NAME

Catalyst::ActionRole::JSV - A JSON Schema validator for Catalyst actions

# SYNOPSIS

    package MyApp::Controller::Course;
    use Moose;
    use namespace::autoclean;

    BEGIN { extends 'Catalyst::Controller'; }

    # RESTful API (Consumes type action) support by Catalyst::Runtime 5.90050 higher
    __PACKAGE__->config(
        action => {
            '*' => {
                Consumes => 'JSON',
                Path => '', 
            }   
        }   
    );


    # Get info on a specific item
    # GET /item/:item_id
    sub lookup :GET Args(1) :Does(JSV) :JSONSchema(root/schema/lookup.json) {
        my ( $self, $c, $item_id ) = @_;
        ...
    }


    # lookup.json (json schema draft4 validation)
    { 
        "title": "Lookup item",
        "type": "object",
        "properties": {
            "item_id": { 
                "type": "integer",
                "minLength": 1,
                "maxLength": 9, 
                "captureargs": 1  # In the case of URL CaptureArgs number 
            }   
        },  
        "required": ["item_id"]
    } 

# DESCRIPTION

Catalyst::ActionRole::JSV is JSON Schema validator for Catalyst actions.
Internally use the json schema draft4 validator called JSV. 

## Error Response

On error it returns 400 http response status. The stash key to set the error message is 'View::JSON expose\_stash' key.
The default key if omitted is 'json'.

    $c->stash->{'View::JSON expose_stash key'} = {message => 'JSV->validate->get_error'}

myapp.yml config

    name: MyApp
    View::JSON:
        expose_stash: 'json'

# SEE ALSO

- [Catalyst::Controller](https://metacpan.org/pod/Catalyst::Controller)
- [Catalyst::View::JSON](https://metacpan.org/pod/Catalyst::View::JSON)
- [JSV::Validator](https://metacpan.org/pod/JSV::Validator)

    Catalyst Advent Calendar 2013 / How to implement a super-simple REST API with Catalyst

    http://www.catalystframework.org/calendar/2013/26
    

# AUTHOR

Masaaki Saito <masakyst.public@gmail.com>

# COPYRIGHT

Copyright 2017- Masaaki Saito

# LICENSE

This library is free software; you can redistribute it and/or modify
it under the same terms as Perl itself.

# SEE ALSO
