## Example SPARQL queries

Select all weights <50kg

```
PREFIX sio: <http://semanticscience.org/resource/>

SELECT DISTINCT ?person ?bodyWeight ?bodyWeightunit { 
	 ?bodyWeightAttribute a <https://loinc.org/29463-7/>;
                       sio:SIO_000217 ?bodyWeightQuality. 
    
    ?bodyWeightQuality a sio:SIO_000005;
                       sio:SIO_000300 ?bodyWeight;
                       sio:SIO_000221 ?bodyWeightunit .
    
    FILTER (?bodyWeight < 50)    
    ?person sio:SIO_000008 ?bodyWeightAttribute    
}
```

Select all heigths >180cm

```
PREFIX sio: <http://semanticscience.org/resource/>

SELECT DISTINCT ?person ?bodyHeight ?bodyHeightUnit { 
	 ?bodyHeightAttribute a <https://loinc.org/8302-2/>;
                       sio:SIO_000217 ?bodyHeightQuality. 
    
    ?bodyHeightQuality a sio:SIO_000005;
                       sio:SIO_000300 ?bodyHeight;
                       sio:SIO_000221 ?bodyHeightUnit .
    
    FILTER (?bodyHeight > 180)    
    ?person sio:SIO_000008 ?bodyHeightAttribute    
}
```

Select all fat-free mass >60kg measured with Bodpod

```
PREFIX sio: <http://semanticscience.org/resource/>
PREFIX obo: <http://purl.obolibrary.org/obo/>

SELECT DISTINCT ?person ?ffm ?ffmUnit ?deviceUsed { 
	 ?ffmAttribute a <http://snomed.info/id/248363008>;
                       sio:SIO_000217 ?ffmQuality. 
    
    ?ffmQuality a sio:SIO_000005;
                       sio:SIO_000300 ?ffm;
                       sio:SIO_000221 ?ffmUnit .
    
    FILTER (?ffm > 60)    
    
    ?ffmAssessment sio:SIO_000332 ?ffmAttribute;
                   sio:SIO_000213 ?deviceUsed .
    
    ?deviceUsed a obo:ERO_0002185 .
    
    ?person sio:SIO_000008 ?ffmAttribute    
}
```

Select from two different triple stores

```
PREFIX sio: <http://semanticscience.org/resource/>
PREFIX obo: <http://purl.obolibrary.org/obo/>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

SELECT DISTINCT ?person { 
	  
    {
    ?person sio:SIO_000008 ?bodyHeightAttribute; 
           sio:SIO_000228 ?patientRole.  
    }
    
    UNION {
    
    
   SERVICE <http://localhost:7200/repositories/Demo_FDP_AUMC> {
        ?person sio:SIO_000008 ?bodyHeightAttribute; 
           sio:SIO_000228 ?patientRole.  
    }
    }
    
    
} #order by ?person
```