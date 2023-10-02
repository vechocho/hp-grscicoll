---
lang-ref: connected-systems
title: Sistemas conectados
description: |
    El Registro Mundial de Colecciones Científicas (GRSciColl) pretende mejorar la interoperabilidad e interactúa con otros sistemas.
    
background:  "{{ site.data.images.echinometra.src }}"
imageLicense: "{{ site.data.images.echinometra.caption }}"
height: 70vh
# layout: documentation
# sideNavigation: about.about
# composition: # you can extend the documentation layout with a custom composition
#  - type: postHeader
#  - type: pageMarkdown
toc: true
---

## Identificadores

Los identificadores y códigos de referencia son esenciales para permitir la interoperabilidad.

Cada entrada de GRSciColl tiene un Identificador Único Universal (UUID) y URLs asociadas. Los [editores](/es/how-to#convertirse-en-editor) también pueden añadir una serie de identificadores externos a sus colecciones y entradas institucionales.
Aquí está la lista actual de tipos de identificadores disponibles:

<ul id="identifierEnums"></ul>

<script>
    // Function to fetch and display data
    function fetchAndDisplayIdentifiers() {
        const url = 'https://api.gbif.org/v1/enumeration/basic/IdentifierType';
        const identifierEnumsList = document.getElementById('identifierEnums');
        fetch(url)
            .then(response => {
                if (!response.ok) {
                    throw new Error(`Network response was not ok: ${response.status}`);
                }
                return response.json();
            })
            .then(data => {
                // Clear any existing list items
                identifierEnumsList.innerHTML = '';
                // Iterate through the array and create list items
                data.forEach(identifier => {
                    const listItem = document.createElement('li');
                    listItem.textContent = identifier;
                    identifierEnumsList.appendChild(listItem);
                });
            })
            .catch(error => {
                console.error('Error fetching data:', error);
            });
    }
    // Call the function to fetch and display data when the page loads
    fetchAndDisplayIdentifiers();
</script>


Además del trabajo de los editores de GRSciCOll, se importaron de forma automática o semiautomática una serie de identificadores para varias instituciones.
* Todas las entradas conectadas al Index Herbariorum reciben un identificador Index Herbariorum. Revise [cómo funciona la sincronización con el Index Herbariorum](/about#index-herbariorum).
* Muchos identificadores `CITES` también proceden del Index Herbariorum. revise [cómo funciona la sincronización con el Index Herbariorum](/about#index-herbariorum)..
* Vinculamos tantas entradas de instituciones GRSciColl como fue posible con [Wikidata](https://www.wikidata.org/) con su herramienta de resolución [OpenRefine](https://openrefine.org) e importamos los identificadores de wikidata.
* Vinculamos tantas entradas de instituciones GRSciColl como fue posible con el [Research Organization Registry (ROR)](https://ror.org) con su herramienta de resolución [OpenRefine](https://openrefine.org) e importamos los identificadores ROR.
* Estamos trabajando con el equipo de [NCBI BioCollection](https://www.ncbi.nlm.nih.gov/biocollections) para importar sus identificadores en GRSciColl. Ya hemos importado los identificadores de varias instituciones.

Las instituciones y colecciones pueden buscarse por identificadores tanto en nuestro sitio web como con [nuestro servicio de búsqueda API](https://www.gbif.org/developer/registry#lookup).

Los identificadores también se utilizan para vincular las ocurrencias relacionadas con especímenes publicadas en GBIF con las entradas de GRSciColl.

## Occurrencias publicadas en GBIF

GBIF interpreta cada nueva ocurrencia publicada. Si esta ocurrencia tiene un valor para cualquiera de los siguientes términos, la interpretación intentará enlazarlo con una entrada GRSciColl usando el servicio de búsqueda de GRSciColl:
* `institutionCode`
* `collectionCode`
* `institutionID`
* `collectionID`

El [servicio de búsqueda de GRSciColl](https://www.gbif.org/developer/registry#lookup) intenta encontrar qué entradas de GRSciColl coinciden con los códigos e identificadores dados como entrada. Durante la interpretación de la ocurrencia, el sistema utilizará el país del editor para ayudar a elegir una coincidencia en GRSciColl en los casos en que haya más de un candid

Por ejemplo, si una ocurrencia hace referencia al código de institución `RBINS` y al identificador de institución `https://ror.org/02y22ws83`, se vinculará al [Real Instituto Belga de Ciencias Naturales](http://grscicoll.org/institution/royal-belgian-institute-natural-sciences).
Puede aprender más sobre cómo vincular ocurrencias de GBIF a GRSciColl [aquí](/es/how-to#how-to-link-specimen-related-occurrences-published-on-gbif-to-grscicoll-entries).

Las ocurrencias vinculadas a GRSciColl se utilizan para generar algunos de los paneles y métricas del sitio web de GRSciColl.

## Datos de GRSciColl procedentes de otras fuentes

Las entradas de instituciones y colecciones de GRSciColl pueden tener una fuente de información primaria externa. En otras palabras, la información disponible en GRSciColl puede proceder de otro registro o sitio web. Cada vez que se edita la fuente primaria, se actualiza también la entrada GRSciColl correspondiente.
El objetivo es evitar mantener la misma información en múltiples registros.

Actualmente, las dos posibles fuentes de información para las entradas GRSciColl son el [Index Herbariorum](https://sweetgum.nybg.org/science/ih/) y los conjuntos de datos de [GBIF](https://www.gbif.org) y los metadatos de los publicadores.
Cuando una entrada está conectada a una de estas fuentes, los datos deben editarse en la fuente. En la práctica, la interfaz de edición no permite actualizar los campos en los que la información procede de una fuente externa.

### Index Herbariorum

Cada semana, GRSciColl se sincroniza con la API del [Index Herbariorum](https://sweetgum.nybg.org/science/ih/). Esto significa que utilizará la información accesible a través de dicha API para actualizar las entradas existentes cuyas fuentes sean el Index Herbariorum, así como para crear nuevas entradas.

Por defecto, una entrada del Index Herbariorum corresponde a una entrada de institución así como a una entrada de colección en GRSciColl. Esto se debe a que los Herbarios son a menudo colecciones de botánica dentro de otras instituciones. Puede leer más sobre la justificación en [este tema de GitHub](https://github.com/gbif/registry/issues/167).
El proceso de sincronización puede generar entradas de instituciones duplicadas, ya que varios herbarios pueden pertenecer a la misma universidad. Consulte nuestra página [how-to](/es/how-to#how-to-use-the-grscicoll-editing-interface) para tratar estos casos.

Los editores pueden desconectar las entradas de instituciones del Index Herbariorum y elegir editar la institución directamente en la interfaz de edición de GRSciColl. Consulte nuestra página [how-to](/es/how-to#how-to-use-the-grscicoll-editing-interface).

### Metadatos de conjuntos de datos de GBIF y páginas de publicadores de GBIF

Los metadatos de los conjuntos de datos publicados en GBIF pueden usarse como fuentes primarias para las entradas de las colecciones en GRSciColl. A diferencia de la sincronización con el Index Herbariorum, no hay un calendario semanal y las nuevas entradas no se crean automáticamente.
En su lugar, los editores deben vincular manualmente las colecciones de GRSciColl con sus fuentes. Esto se debe a que el ámbito de GBIF incluye datos que van más allá del ámbito de GRSciColl. Tenga en cuenta que también existe la opción de crear una entrada de colección a partir de un conjunto de datos. Vea nuestra página [how-to](/es/how-to#how-to-use-the-grscicoll-editing-interface).
Cuando se actualizan los metadatos de un conjunto de datos, la entrada de colección correspondiente se actualiza inmediatamente.

Del mismo modo, la información sobre publicadores disponible en GBIF puede utilizarse como fuente primaria para las entradas de instituciones en GRSciColl.

## GRSciColl as content for other websites

The [GRSciColl API](/api) makes it possible for other applications to access the GRSciColl data programmatically. This means that other website are able to display the GRSciColl conten, which remains centrally curated. Anyone can use the API to include the GRSciColl data in their systems. Below are two documented examples.

### iDigBio

The data displayed on the [iDigBio Collections website](https://www.idigbio.org/portal/collections) is maintained in GRSciColl. iDigBio is part of our team of editors and review update suggestions for US institutions.

### GBIF Hosted portals

The current GRSciColl website is in fact one of the [GBIF Hosted Portals](https://www.gbif.org/hosted-portals). Any GBIF Hosted portal can display the GRSciColl data. See for example [the UK Natural Sciences Collections Portal](https://data.dissco-uk.org). 