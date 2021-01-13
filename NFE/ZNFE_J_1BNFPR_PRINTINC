*&---------------------------------------------------------------------*
*&  Include           J_1BNFPR_PRINTINC
*&---------------------------------------------------------------------*


  TABLES: J_1BPRNFHD,                  "document header
          J_1BPRNFIS,                  "issuer data
          J_1BPRNFFA,                  "fatura data
          J_1BPRNFLI,                  "line item data
          J_1BPRNFST,                  "subtotals across pages
          J_1BPRNFTR,                  "carrier data
          J_1BPRNFRE,                  "Bill-to party data
          J_1BPRNFRG,                  "payer data
          J_1BPRNFTX,                  "texts
          J_1BPRNFDE,                  "destination data
          J_1BSTAST,
          J_1BINNAD,
          J_1BBRANCH,
*          ZZJ_1BPRNFLI_TOT,
          T052,
          T052S.                       "Reg. Tax code for Sub.Trib.


TABLES: J_1BAGNT.                     "CFOP description
TABLES: *J_1BNFDOC.


DATA:   TAX_TYPES LIKE J_1BAJ OCCURS 30 WITH HEADER LINE.

DATA: TABIX    LIKE SY-TABIX,
          TOTLINES(2) TYPE N.

*... table: terms of payment - texts ..................................*

DATA: BEGIN OF ZTEXT OCCURS 4.
        INCLUDE STRUCTURE TTEXT.
DATA: END OF ZTEXT.

*Data for terms of payment
*... table: terms of payment ..........................................*



*... internal table: terms of payment - holdback/retainage ............*

DATA: INT_T052S LIKE T052S OCCURS 0 WITH HEADER LINE.

*... data for terms of payment ........................................*

DATA: T052SLINES(2) TYPE N,
      RATE          LIKE J_1BPRNFFA-RATPZ1,
      TEXT1         LIKE J_1BPRNFFA-TXT11,
      TEXT2         LIKE J_1BPRNFFA-TXT12,
      TEXT3         LIKE J_1BPRNFFA-TXT13,
      TEXT4         LIKE J_1BPRNFFA-TXT14.
*End Data for terms of payment

*... data for text and cfop determination .............................*

DATA: SEQNUM        LIKE J_1BPRNFTX-SEQNUM,  "sequence number of text
      MESSAGE       LIKE J_1BPRNFTX-MESSAGE, "text
      OLD_SEQNUM    LIKE J_1BPRNFTX-SEQNUM,  "last sequence number
      ISTART        LIKE SY-TABIX.
DATA: CFOP_CHARACTER1(1)  TYPE C,    "one byte of CFOP
      CFOP_CHARACTER2(1)  TYPE C,    "one byte of CFOP
      CFOP_POSITION       LIKE SY-INDEX, "position in CFOP
      CFOP_CHAR10(10)      TYPE C.
DATA:
      BEGIN OF S_CFOP_CHAR10,
      S0(1),
      S1(1),
      S2(1),
      S3(1),
      S4(1),
      S5(1),
      S6(1),
      S7(1),
      S8(1),
      S9(1),
      END OF S_CFOP_CHAR10.
DATA:
       BEGIN OF S_J_1BPRNFHD_CFOP,
       S0(1),
       S1(1),
       S2(1),
       S3(1),
       S4(1),
       S5(1),
       S6(1),
       S7(1),
       S8(1),
       S9(1),
       END OF S_J_1BPRNFHD_CFOP.
*End data for text and cfop determination


*... work fields to read issuer information ...........................*
DATA: BEGIN OF ISSUER,
        PARTNER_TYPE     LIKE J_1BNFNAD-PARTYP,
        PARTNER_ID       LIKE J_1BNFNAD-PARID,
        PARTNER_FUNCTION LIKE J_1BNFNAD-PARVW,
      END   OF ISSUER.

*... work fields to read destination information ......................*
DATA: BEGIN OF DESTINATION,
        PARTNER_TYPE     LIKE J_1BNFNAD-PARTYP,
        PARTNER_ID       LIKE J_1BNFNAD-PARID,
        PARTNER_FUNCTION LIKE J_1BNFNAD-PARVW,
      END   OF DESTINATION.



*... help fields for handling of table INTER_TOTAL_TABLE_ADOBE .........




CLEAR:    J_1BPRNFHD,                  "document header
*              J_1BPRNFIS,                  "issuer data
          J_1BPRNFFA,                  "fatura data
          J_1BPRNFLI,                  "line item data
          J_1BPRNFST.                  "subtotals across pages
*              J_1BPRNFTR,                  "carrier data
*              J_1BPRNFRE,                  "bill-to party data
*              J_1BPRNFRG,                  "payer data
*              J_1BPRNFTX,                  "texts
*              J_1BPRNFDE.                  "destination data

DATA:     cfop_version      TYPE j_1bcfop_ver,         " note 593218
          item_cfop_long    TYPE j_1bcfop_long,
          cfop_length       TYPE j_1bcfop_len,         " note 593218
          extension_length  TYPE j_1bcfop_len,
*              reallength        TYPE j_1bcfop_len,
          defaulttext       TYPE j_1bcfop_txtdef,
          cfop_lines        TYPE n,                    " note 593218
          issuer_region     TYPE regio,                " note 593218
          ENCODED_CFOP      TYPE J_1BCFOP_LONG.
