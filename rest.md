# URN (Uniform Resource Name)
### URN is a wrapper for different "unique identifier" schemes


# URL (Uniform Resource Location)
### URL specifies the location of a resource

- "http://..."
- "ftp://..."
- "mailto:...".

# URI (Uniform Resource Identifier)
### URI is a URL or a URN
So, to fully understand what URI means, you need to first understand what is a URN.

URN is an acronym for uniform resource name. There are may "unique identifier" schemes in the world, for example, ISBNs (globally unique for books), social security numbers (unique within a country), customer numbers (unique within a company's customers database) and telephone numbers. Each "unique identifier" scheme has its own notation. A URN is a wrapper for different "unique identifier" schemes. The syntax of a URN is "urn:<scheme-name>:<unique-identifier>". A URN uniquely identifies a resource, such as a book, person or piece of equipment. By itself, a URN does not specify the location of the resource. Instead, it is assumed that a registry provides a mapping from a resource's URN to its location. The URN specification does not state what form a registry takes, but it might be a database, a server application, a wall chart or anything else that is convenient. 

Some hypothetical examples of URNs are
- "urn:employee:08765245"
- "urn:customer:uk:3458:hul8"
- "urn:foo:0000-0000-9E59-0000-5E-2"

The <scheme-name> ("employee", "customer" and "foo" in these examples) part of a URN implicitly defines how to parse and interpret the <unique-identifier> that follows it. An arbitrary URN is meaningless unless: (1) you know the semantics implied by the <scheme-name>, and (2) you have access to the registry appropriate for the <scheme-name>. A registry does not have to be public or globally accessible. For example, "urn:employee:08765245" might be meaningful only within a specific company.
To date, URNs are not (yet) as popular as URLs. For this reason, URI is widely misused as a synonym for URL.
IRI is an acronym for internationalized resource identifier. An IRI is simply an internationalized version of a URI. In particular, a URI can contain letters and digits in the US-ASCII character set, while a IRI can contain those same letters and digits, and also European accented characters, Greek letters, Chinese ideograms and so on.