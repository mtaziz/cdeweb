# Reading Documents

Most commonly, you want to pass an entire document file to ChemDataExtractor.

ChemDataExtractor comes with a number of built-in Document readers that can read HTML (web pages), PDF and XML files:

    >>> f = open('paper.html')
    >>> doc = Document.from_file(f)

Each reader will be tried in turn until one is successfully able to read the file. If you know exactly which readers
you want to use, it is possible to specify a list as an optional `readers` parameter:

    >>> f = open('rsc_article.html')
    >>> doc = Document.from_file(f, readers=[RscDocumentReader])

The available readers are:

- AcsHtmlDocumentReader - For ACS HTML articles
- RscHtmlDocumentReader - For RSC HTML articles
- NlmXmlDocumentReader - For NLM/JATS XML (e.g. from PubMed Central)
- XmlDocumentReader - Generic XML
- HtmlDocumentReader - Generic HTML
- PdfDocumentReader - Generic PDF
- PlainTextDocumentReader - Generic plain text

The HTML and XML readers can determine document structure such as headings, paragraphs, and tables with high accuracy.
However, this is much harder to achieve with the PDF and plain text readers.

## Document Elements

Once read, documents are represented by a single linear stream of elements:

    >>> doc.elements
    [Title('A very important scientific article'),
     Heading('Abstract'),
     Paragraph('The first paragraph of text...'),
     ...]

Element types include Title, Heading, Paragraph, Citation, Table, Figure, Caption and Footnote. You can retrieve a
specific element by its index within the document:

    >>> para = doc.elements[14]
    >>> para
    Paragraph('1,4-Dibromoanthracene was prepared from 1,4-diaminoanthraquinone. 1H NMR spectra were recorded on a 300 MHz BRUKER DPX300 spectrometer.')


You can get the individual sentences of a paragraph:

    >>> para.sentences
    [Sentence('1,4-Dibromoanthracene was prepared from 1,4-diaminoanthraquinone.', 0, 65),
     Sentence('1H NMR spectra were recorded on a 300 MHz BRUKER DPX300 spectrometer.', 66, 135)]

Or the individual tokens:

    >>> para.tokens
    [[Token('1,4-Dibromoanthracene', 0, 21),
      Token('was', 22, 25),
      Token('prepared', 26, 34),
      Token('from', 35, 39),
      Token('1,4-diaminoanthraquinone', 40, 64),
      Token('.', 64, 65)],
     [Token('1H', 66, 68),
      Token('NMR', 69, 72),
      Token('spectra', 73, 80),
      Token('were', 81, 85),
      Token('recorded', 86, 94),
      Token('on', 95, 97),
      Token('a', 98, 99),
      Token('300', 100, 103),
      Token('MHz', 104, 107),
      Token('BRUKER', 108, 114),
      Token('DPX300', 115, 121),
      Token('spectrometer', 122, 134),
      Token('.', 134, 135)]]