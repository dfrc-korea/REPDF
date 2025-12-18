# REPDF: Repairing Corrupted PDF Files through Font Mapping and Object Relationship Reconstruction

REPDF is a tool designed to repair corrupted PDF files by using font mapping with an external font database and reconstructing residual object relationships.

The tool is available at the following link: https://repdf.site/

## Datasets
### Defining Corruption Scenarios
To evaluate the performance of REPDF, we define ten real-world PDF corruption scenarios as follows:
| ID | Type&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;| Description | Implementation |
| :--- | :--- | :--- | :--- |
| C1 | Header Corruption | damages the header at the beginning of the PDf file | overwrites up to 12 bytes of the PDF header with random values |
| C2 | XRef table removal | damages the cross-reference(XRef) table which stores the starting byte offsets of indirect objects | removes the cross-reference (XRef) table that precedes the trailer |
| C3 | Trailer section damage | damages the trailer at the end of the PDF file | removes the end-of-file trailer by identifying the startxref pointer |
| C4 | Page tree broken | damages the relationships among page objects | corrupts the page tree by deleting a random-length span around the /Pages, /Kids, and /Count entries |
| C5 | Obejct tag removal | causes a rendering failure by making some objects lose their object numbers | randomly selects an object and removes its header, including the object number and the obj keyword |
| C6 | Font mapping loss | corrupts references to font objects | deletes the /Font dictionary inside a page’s /Resources, breaking the mapping between the /Page object and referenced /Font objects |
| C7 | Font stream deletion | removes embedded font binaries and their references | deletes the /FontFile2 stream data |
| C8 | Font resources deletion | removes embedded font binaries, their references, and Unicode mapping information | deletes the /FontFile2 stream data and /ToUnicode stream data |
| C9 | Zlib tampering | trigger decoding errors by byte-level modification within zlib-compressed streams | corrupts compressed stream data by modifying a random byte within a zlib-compressed stream |
| C10 | Partial truncation | corrupts the latter part of the file | truncates the file by cutting off the end of the file, retaining approximately 70% of the original bytes |

### Dataset Construction
The content for each language was automatically generated using the GPT-5 model to ensure grammatical accuracy and a natural tone. 
The dataset includes six languages: English, French, Spanish, Arabic, Hindi, and Chinese, that represent alphabetic, syllabic, and logographic writing systems with varying character encoding. 
To support cross-lingual evaluation, each document comprises six pages, with identical content presented in a different language on each page.
- **Original Files**
  - We used Microsoft Word and then converted 50 base files into individual PDF files using the “Save As” and “Print to PDF” options, resulting in a total of **100 generated documents**. These methods reflect commonly used document creation workflows in real-world environments.
    
  - Paths (# of Documents)
    - `original/print/text(40)`
    - `original/print/text+img(10)`
    - `original/saveas/text(40)`
    - `original/saveas/text+img(10)`

- **Corrupted Files**
  - According to the corruption scenarios, **1,000 PDF files** (50 base files × 10 damage types × 2 creation methods—Save As and Print to PDF) were generated and used in the experiments. Each corruption type can be identified by the suffix specified in the table below.
  - Paths (# of Documents)
    - `corrupted/print/text(400)`
    - `corrupted/print/text+img(100)`
    - `corrupted/saveas/text(400)`
    - `corrupted/saveas/text+img(100)`
  - Summary of dataset according to the corrupted scenarios
    
    | ID | Type&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;| Suffix | # of Documents(Text only) | # of Documents(Text+Image) |
    | :--- | :--- | :--- | :--- | :--- |
    | C1 | Header Corruption | _header | 40 (saveas) / 40 (print) | 10 (saveas) / 10 (print) |
    | C2 | XRef table removal | _xref | 40 (saveas) / 40 (print) | 10 (saveas) / 10 (print) |
    | C3 | Trailer section damage | _trailer | 40 (saveas) / 40 (print) | 10 (saveas) / 10 (print) |
    | C4 | Page tree broken | _page_tree | 40 (saveas) / 40 (print) | 10 (saveas) / 10 (print) |
    | C5 | Obejct tag removal | _object_header | 40 (saveas) / 40 (print) | 10 (saveas) / 10 (print) |
    | C6 | Font mapping loss | _font_mapping_loss | 40 (saveas) / 40 (print) | 10 (saveas) / 10 (print) |
    | C7 | Font stream deletion | _remove_fonts | 40 (saveas) / 40 (print) | 10 (saveas) / 10 (print) |
    | C8 | Font resources deletion | _remove_unicode_fonts | 40 (saveas) / 40 (print) | 10 (saveas) / 10 (print) |
    | C9 | Zlib tampering | _stream_zlib | 40 (saveas) / 40 (print) | 10 (saveas) / 10 (print) |
    | C10 | Partial truncation | _partial_cut | 40 (saveas) / 40 (print) | 10 (saveas) / 10 (print) |

## Evaluation
The evaluation was performed on a dataset of 1000 corrupted files, assessing text recovery rates for each language and the success of image recovery and rendering. 

While content rendering and error handling can vary across PDF viewers due to the PDF format's flexibility, the built-in viewer in the Google Chrome Web Browser was used for all assessments.

