# vrm

ORMs are a weird abstraction. They map concepts from relational databases
(namely, records from a table, with fields corresponding to columns) to
*objects* in the target programming language. They make is easy for programmers
to shoot themselves in the foot by inadvertently triggering bad queries, or too
many queries. Meanwhile, objects are not the point of software anyway. They're
a tool for programmers to ultimately deliver some value to an end user.

A _view_ is closer to what software is all about. It's a presentation of data
intended for human comprehension. So the premise here is to cut out the middle
man (objects) and provide an abstraction that goes straight from a relational
database to a view of the data. The mapping layer can be smart enough to always
execute exactly the right queries to provide the view.

## examples

This is me thinking out loud. If/when this thing actually exists in anything
resembling a usable form, I can provide better examples.

But let's say this is the view I want to present:

```
[{
    'user': {
        'username': user.username,
        'name': user.name
    },
    'repositories': [{
        'name': repository.name,
        'description': repository.description
    } for repository in user.repositories]
} for user in users[:10]]
```

This could produce some SQL like:

```
SELECT username, name
FROM users u
LEFT JOIN repositories r ON r.user_id = u.id
LIMIT 10;
```

The inference of the need for `LEFT JOIN` is an example of the kind of thing
this library would take care of. (In a more traditional ORM scenario, accesing
`user.repositories` would trigger an additional query, unless the developer
had been smart enough to add a `select_related` or `prefetch_related` or
equivalent.)
