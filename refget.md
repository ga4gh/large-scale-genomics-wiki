# Refget: access to reference sequences

## What is refget?

Refget is a specification to define a **standard way to access reference sequences** using an **identifier system derived from the sequence itself**. It is a fundamental building block of GA4GH providing a way for our standards to access sequences and to unambiguously identify them.

![How refget works](refget-big.png)

## How do I use refget?

### Getting sequence

Refget is a HTTP based standard so you can access sequences using any HTTP library. All you need to access a refget service is a HTTP library and a known identifier. The following Python code retrieves the first 10 bases of _Saccharomyces cerevisiae_ chromosome I (MD5 identifier `6681ac2f62509cfc220d78751b8dc524`).

```python3
import requests

checksum = "6681ac2f62509cfc220d78751b8dc524"
server = "https://www.ebi.ac.uk/ena/cram/sequence"
url = f"{server}/{checksum}"
r = requests.get(url, params={'start':0, 'end':10})
print(r.text)
'CCACACCACA'
```

Omit the start and end parameters to retrieve the entire sequence.

### Getting sequence metadata

```python3
import requests

checksum = "6681ac2f62509cfc220d78751b8dc524"
server = "https://www.ebi.ac.uk/ena/cram/sequence"
url = f"{server}/{checksum}/metadata"
r = requests.get(url)
print(r.json())
{'metadata': {'aliases': [{{'alias': 'I', 'naming_authority': 'unknown'}], 'length': 230218, 'md5': '6681ac2f62509cfc220d78751b8dc524'}}
```

## Refget Sequence Collections

An GA4GH approved product called [Refget Sequence Collections has been released from the GA4GH Large Scale Genomics workstream](https://ga4gh.github.io/refget/seqcols/). This standard allows the same mechanisms as Refget Sequences to operate over groups of sequences such as those composing an assembly, transcriptome or proteome. 

## Essential refget information

### Specifications

- [Refget's reference specification](https://ga4gh.github.io/refget/sequences/)
- [Refget in OpenAPI](https://ga4gh.github.io/refget/sequences/pub/refget-openapi.yaml)

### Compliance

- [Compliance document](https://compliancedoc.readthedocs.io/en/latest/)
- [Compliance suite](https://github.com/ga4gh/refget-compliance-suite)

### Implementations

- [CRAM Reference registry](https://www.ebi.ac.uk/ena/cram) deployed by ENA at EMBL-EBI. Provides access to all submitted INSDC sequence via the refget standard
- [Ensembl Refget](https://beta.ensembl.org/data/refget/) access to reference sequences including genomic, transcripts and proteins. Running implementation of the [Ensembl Refget implementation](https://github.com/Ensembl/ensembl-refget).
- [Refget reference implementation](https://github.com/andrewyatz/refget-server-python) written in Python

## What does an identifier derived from the sequence mean?

Sequences such as reference genomes have a multitude of names. For example chromosome 1 from the latest build of the human genome (GRCh38) can be known as `chr1`, `1`, `CM000663.2` or `NC_000001.11`  depending on where you accessed your sequence from. Refget instead uses a [cryptographic hash function](https://en.wikipedia.org/wiki/Cryptographic_hash_function) to create an identifier based on the sequence content by digesting the A,C,G and Ts from a chromosome and passing it through the MD5 or SHA512 algorithm creating a string. Chromosome 1 can now be referred to as `6aef897c3d6ff0c78aff06ac189178dd`.

## How to get involved

You can contribute changes to the [hts-specs GitHub repository](https://github.com/ga4gh/refget). If you want to be more involved we host regular conference calls. More details can be found at our [GA4GH landing page](https://www.ga4gh.org/product/refget/).
