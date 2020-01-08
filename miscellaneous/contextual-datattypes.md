# Contextual Data types & functions
This is a thought & discussion paper.  It captures evolving thinking around how 
to make highly contextual data discoverable, and explores when metadata 
should / shouldn't be included within the taxonomy.

We then explore implementation and language considerations, with a specific look 
towards evolving the Taxi language.

[TOC]

# Executive Summary
Discoverability is underpinned by rich, well-defined taxonomies which define 
both the Types of data that exist across the organisation, and their surrounding 
Context.  

Context can be broken into (at least) two categories:
 - Structural (which help refine the definition & meaning of the data type itself)
   - These should be present within the Taxonomy
 - Environment (which define the conditions under which data was produced)
   - These should not be present within the Taxonomy, but are expressed in terms from the taxonomy.

## Structural context - Traits
Existing as part of the type system, Traits are a term we use to define 
commonalities identified across many other types.  They're generally not types 
themselves, but a way of labelling recurring themes within the taxonomy.  
`QuotablePrice` and `Markup` are examples of traits -- not types themselves 
(as they're not sufficiently specific), but a recurring theme in the context of
of other types.

Traits are first-class citizens within the taxonomy, and should be defined and 
catalogued.  We should take care to differentiate them from Types. On their 
own, Traits are not specific enough to be useful, so they require a 
relationship to a type to give them context.  When traits are added (or mixed-in) 
to another type, they add useful colour & context.

In the land of Object Oriented programming, Traits are sometimes present as 
their own concept (eg., Scala), but are generally most similar to Interfaces, 
and specifically marker interfaces (ie., interfaces that don't define any 
structure, but are simply used to add meaning or functionality to an underlying 
type)

## Environment

Environmental context helps us define the conditions under which data was 
produced.  It's the 'When', 'Where' and 'Who' surrounding the data, (whereas 
Types and Traits help categorize the 'What').

For discoverability of the *right* data, it's important to be able to express 
requirements in both Structural and Environmental context -- that is, 
(meta?) data that satisfies both the 'What' and the 'When'.

Environment is highly fluid and often the possible values are not easily 
constrained by a taxonomy.  Populating such a taxonomy with a list 
of all the conditions data was provided isn't practical.  However, the terms 
that we use to define the Environment **do** belong in the Taxonomy.

Environmental Context is best expressed as **one or both** of:
 - Metadata about the data set 
 - Contracts on services that expose this data.

The terms used *within* the Environment Context *should* be defined 
within the Taxonomy as concepts -- For example, `ReportProducer` and 
`PublicationDate`. These concepts need to be defined, curated and applied with 
the same care as any other taxonomy entry.

Environmental terms can then be applied on operations, to help refine the 
context of data that is returned, increasing discoverability of the right 
data.  To enable us to define a query not just based on one or more of the 
following properties:
 * the data content and type    
 * the way the data was produced 

The key is to apply the same data type rigour to our treatment of the origins
 and metadata surrounding the data as we do to the data itself.

Environmental Context is ultimately expressed as a series of Triples - `
Report123 wasProducedBy Bloomberg`.   This is sometimes referred to as 
`Subject-Predicate-Object` - though the most useful distinction is that the 
middle part of the triple is a `Predicate` - a way of applying criteria to other 
terms within the taxonomy.  i.e the values accepted by the `wasProducedBy` 
proedicate can be values within our taxonomy comprising of {'Bloomberg', 'Reuters', 
'Moodys'}. 

Later, we'll explore options for how different languages and taxonomies handle 
these, including Taxi.

# A Worked example

Consider the following requirement:

 > Discover QuotedPrices within a time range, from one or all markets, for a given ISIN.

 First, let's formalise this a little, and identify the terms in use, 
with inputs marked `${likeThis}`:

  - Discover `QuotedPrices`
  - Where the `QuoteTime` is `between` `${StartDate}` and `${EndDate}`
  - and the `PublicationSource` is `one of` `${aListOfMarkets}`
  - and the `ISIN` is `equalTo` `${someIsin}`

Here, we can see the following Taxonomy terms:

 - `QuotedPrice` is a Trait, applied to a more specific Type (`FxAllInPrice`).  
    Using the Trait to specify our requirement, rather than the specific 
    concrete type is useful, as the report may also contain `BondAllInPrice`.
 - `QuoteTime`, `ISIN` and `PublicationSource` are types within the hierarchy
 - `${StartDate} ${EndDate} ${aListOfMarkets}` and `${SomeIsin}` are inputs.  
    These aren't members of the taxonomy.
 - `between` `${StartDate}` and `${EndDate}`, `PublicationSource` is `one of` `${aListOfMarkets}`, and `ISIN` is `equalTo` `${someIsin}` are all Environmental Context.

**Why would `${someIsin}` not be part of the taxonomy?...surely that's a known 
list of values??**

Here, we can see the Environment context itself (`QuoteTime`, `Isin` and 
`PublicationSource`) is not something that is added to the Taxonomy, but the 
terms that define it are.

 > Note:  Deciding when something is a Type vs a Trait is an artform, for 
 > which there are no firm answers.  We'll explore useful language tools for 
 > determining later, but there's no hard and fast rule.

# A deeper dive

## This is a work in progress
From this point on, we're delving into WIP territory.  The document isn't 
finished yet, and this represents partially formed thoughts.  This is 
predominantly about how to express the above requirements in Taxi, and 
further tease out ideas explored earlier.

You, dear reader, are welcome to read and comment.  But here be demogorgons.


## Traits vs Context -- What, vs When, Where & Who
In defining & refining data types, we try to add specificity in the type 
definition, to reduce ambiguity.

Traits -- Reducing ambiguity in “What”

```
Price -> FxPrice -> NearLegPrice -> TraderNearLegPrice
                                 -> SalesNearLegMarkup
                                 -> ClientNearLegPrice
```

One could liken Traits to User Defined Types that are re-used across Types in
the hierarchy but it is their parent type that gives them context.

Is it true to say one could achieve the same functionality by never using 
traits, and only ever using types.  However Traits allow us to DRY up the 
declaration and share the commonality.

Alternatively, Traits are declarations of data + type info and they are 
"instantiated" in a Type.

Traits are useful to refine the meaning of the data, and enriching the 
categorisations of the data within a broader taxonomy.  

Traits tend to be heirarchical, but each subtype may belong to multiple 
independent hierarchies.

For example:

```
ClientNearLegPrice has trait NearLegPrice, ClientPrice
SalesNearLegMarkup has trait NearLegPrice, SalesMarkup

Price -> Markup -> SalesMarkup
```

Each layer within the heirachy is a specific Trait, which enriches our 
understanding about the idea the data is trying to communicateand represent.  
All FxPrices are Prices, but not all Prices are FxPrices.


These traits remain true regardless of where we discovered the data from -- 
A `ClientNearLegPrice` is always a `ClientPrice` regardless of it was provided 
by an RFQ or in an executed trade, and published/created in January or November.

Here, `ClientPrice` is used to provide context about how this information 
may be used -- for example, it’s appropriate for sharing with Clients, 
whereas other types (such as the markup or trader price) should not be shared
with clients.

Traits of data form an important part of the definition of the DataType.  Each 
trait itself belongs within the taxonomy, and should have a crisp definition.


## Context - Where, when, who.
Context provides the parameters under which data has been provided.  It’s not 
always necessary to provide context - some data is self explanatory.  (A client 
record), however context is often useful.  Especially so when we start to 
think about data lineage and provenance.

Context itself does not belong in the taxonomy - it’s metadata about other 
information.  It isn’t useful to fill the taxonomy with a unique entry to model 
that a certain report contains data from a specific provider, for a certain 
date range, for a given set of entities.

Polluting the taxonomy with data detailing the changing landscape of data 
production is an anti-pattern and will tie the business taxonomy to the IT 
infrastructure that created it.  The taxonomy should stand alone as a series 
of business or real-world concepts that enrich our discussion of the problem 
domain, not used to describe or enumerate the source systems that generated or
serve it. 

However, the terms that we use to define the context ***do*** belong in the 
taxonomy.  Generally, context is defined as a condition against an entity or 
attribute -- “quote date is between these dates”, “for a specific ISIN”, 
“originated from a specific publisher”.    The attributes we’re constraining 
are certainly taxonomy entries.

Some forms of taxonomy also choose to model the conditions themselves - SPARQL 
and RDF define these as URI’s, SQL has a grammar for defining the conditions.  
While having a shared definition of the conditions is useful, it’s unlikely to 
have value within an editable taxonomy tool - simply ensuring that everyone 
agrees on what grammar is being used to define the concepts is enough.

## Context in Contracts
Context is critical for discovery.  Allowing systems to describe the context 
of the data they can provide is paramount for allowing discovery containers 
to know when & how to call data providers.

```
type Price {
   value : MoneyValue as Decimal
   ccy : CurrencySymbol as String
}*


type PriceTick {
   timestamp : QuoteTimestamp as Instant,
   price : Price
}

type FxPrice inherits Price
type NearLegPrice inherits FxPrice
type OutrightPrice inherits 


trait FxPrice
trait ClientPrice

```

Let's explore a service function that provides us with prices.


```
operation findPrices(
   from:QuoteTimestamp,
   to:QuoteTimestamp,
   markets: List<Market>,
   isin:Isin) : List<PriceTick>
```

This example has a contract, and we can infer the meaning of the data that's 
returned, but it's certainly not explicit.  

 - What is the relationship of the list of prices to the markets provided?
 - We can infer from the names used for the timestamps (`from` and `to` )that
  our prices will be between these two dates.

However, this isn't enough to make the data discoverable - we need to be more 
explicit in the contract on the service.

Let's start with the timestamp, as that's easy:

```
operation findPrices(
   from:QuoteTimestamp,
   to:QuoteTimestamp,
   markets: List<Market>,
   isin:Isin) : List<PriceTick>(
       	timestamp ( between from and to )
   )
```

> (i) In Taxi, we use parenthesis `()` to describe contracts, and braces `{}` to 
  describe structure.

**There's no `{}` braces in the above example**

Here, we've said that the returned value (a `List<PriceTick>`) will have 
a `QuoteTimestamp` between the `from` and `to` inputs.

We can now leverage this to explicitly query for prices between two times:

```
// Query:
{
   PriceTick[]( timestamp: between '2019-05-23T08:00:00Z' and '2019-05-23T09:00:00Z' )
}    
```

### Relationships beyond attributes
Not all context can be neatly expressed as a property of the return type.

For example, what's the relationship fo the Isin to the returned PriceTicks?  We 
can intuit that the prices are for the bond indicated by the Isin.  However, 
that's not very explicit.

This is a bit tricky though, because - unlike Timestamp - Isin isn't an attribute on Price.

We need a way to define the relationship.  We can express relationships 
in Taxi using the `->` symbol.  There's a few different ways to express 
the relationship between the Price and the Isin:

 - By mapping that Isins can have prices:
```
type Isin as String {
  hasPrice -> Price[]
}
```

 - By extending the definition of Price to add a relationship to Isin

```
type extension Price {
    isPriceFor -> Isin
}
```

The Price to ThingThatCanBePriced relationship is pretty common, so we may wish 
to express this more generically:

```
trait Priceable {
    hasPrice -> Price
}

type Isin as String has trait Priceable
```

Let's assume we're using the `Isin -[hasPrice]-> Price` relationship.

**This infers we need to define the relationship between price and isin in 
both the isin and the price - am I reading that right?**

Now this is defined, we can add the context into the return contract of our operation:

```
operation findPrices(
    from:StartTimestamp as QuoteTimestamp,
    to:EndTimestamp as QuoteTimestamp,
    markets: Market[],
    isin:Isin) : Price[] {
        timestamp : QuoteTimestamp( between from and to ),
        ( isin -[hasPrice]-> this )
    }
```

Note, if we'd chosen to model our relationship the other way (`Price -[isPriceFor]-> Isin`), it'd look something like this:

```
operation findPrices(
    from:StartTimestamp as QuoteTimestamp,
    to:EndTimestamp as QuoteTimestamp,
    markets: Market[],
    isin:Isin) : Price[] {
        timestamp : QuoteTimestamp( between from and to ),
        ( this -[isPriceFor]-> isin )
    }
```

```
given {
    myIsin:Isin = 'XS1103286305`
} 
discover {
    PriceTick[]( 
        timestamp: between '2019-05-23T08:00:00Z' and '2019-05-23T09:00:00Z',
        this -[isPriceFor]-> myIsin
    )
}
```

We can simplify this down by omitting the Given block, since we're working 
directly with known variables:

```
discover {
    PriceTick[]( 
        timestamp: between '2019-05-23T08:00:00Z' and '2019-05-23T09:00:00Z',
        this -[isPriceFor]-> 'XS1103286305`
    )
}
```

// TODO
 - Not sure that the relationship is fully defined between the Price and the Isin.
 - Tidy up the use of Traits vs interface relationships
 - Explore & offer advice on when a type is a Type vs a Trait
 - Explore whether relationships can be reliably inferred at the language level, much like a human would do.

I wonder if perhaps the paper is in too greater depth.  i think possibly it 
should start from a problem statement and then drilldown into the way Taxi 
solves it.

Maybe it's 3 papers?

problem : how to describe the source/provenance/lineage of data
solution: taxonomy for relationships, not other stuff
..worked example
..how
..why
..why not?

problem : repeating stanzas in data descriptions/user defined type
solution: traits
..worked example
..how
etc.

problem : finding data across the enterprise
solution: taxi's data query language that combines traits, types and meta 
information <== the definition of the surrounding data around a type needs a 
name (i.e. your Vyne docs need a taxonomy!...Drink your own koolaid, Pitt!)
..worked example etc 


