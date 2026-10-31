# Card Details Validation Program (CARDPGM01)

A beginner-friendly COBOL practice program that accepts credit-card
details, validates each record, checks for a repeated card number, and
displays valid records with input and output totals.

## Project Overview

`CARDPGM01` demonstrates a paragraph-based COBOL program structure and
basic record-processing logic.

For each card record, the program accepts:

  -----------------------------------------------------------------------
  Field                   COBOL picture           Description
  ----------------------- ----------------------- -----------------------
  `CARD-NUM`              `PIC X(5)`              Five-character card
                                                  number

  `CARD-NAME`             `PIC X(10)`             Cardholder name

  `CARD-LIMIT`            `PIC S9(5)`             Signed numeric card
                                                  limit

  `CARD-DUE`              `PIC S9(5)V9(2)`        Signed numeric due
                                                  amount with two implied
                                                  decimal places
  -----------------------------------------------------------------------

## Features

-   Accepts card details interactively using `ACCEPT`.
-   Counts the number of input records.
-   Validates that the card number and cardholder name are not blank.
-   Rejects negative card limits and due amounts.
-   Checks whether the current card number matches the previously
    accepted valid card number.
-   Moves valid details to an output record.
-   Displays valid records and input/output totals.

## Program Structure

The program is organized into paragraphs:

  -----------------------------------------------------------------------
  Paragraph                           Responsibility
  ----------------------------------- -----------------------------------
  `000-MAIN-PARA`                     Controls the program flow and
                                      processing loop.

  `1000-INITIAL-PARA`                 Initializes working fields and
                                      counters.

  `2000-INPUT-PARA`                   Accepts card details and increments
                                      the input counter.

  `3000-PROCESS-PARA`                 Coordinates validation, duplicate
                                      checking, output processing, and
                                      continuation input.

  `3100-AUDIT-PARA`                   Validates required fields and
                                      numeric values.

  `3110-DUP-CHECK-PARA`               Checks the card number against
                                      `WS-DUP-CHECK`.

  `3200-MOVE-PARA`                    Increments the output counter,
                                      saves the accepted card number,
                                      moves the record, and displays it.
  -----------------------------------------------------------------------

## Working-Storage Fields

  -----------------------------------------------------------------------
  Field                               Purpose
  ----------------------------------- -----------------------------------
  `WS-VALID-INPUT`                    Uses `Y` or `N` to indicate whether
                                      the current record is valid.

  `WS-INREC-COUNT`                    Counts records accepted as input.

  `WS-OUT-REC-COUNT`                  Counts records accepted for output.

  `WS-END-INPUT`                      Uses `Y` to stop processing and `N`
                                      to continue.

  `WS-DUP-CHECK`                      Stores the most recently accepted
                                      valid card number for duplicate
                                      comparison.
  -----------------------------------------------------------------------

## Duplicate-Check Limitation

The current implementation compares the input card number only with the
value stored in `WS-DUP-CHECK`. Since that field is updated after a
valid record is processed, this detects a repeated card number only when
it matches the immediately preceding valid record.

For example:

-   `12345`, followed by `12345` --- detected as a duplicate.
-   `12345`, followed by `67890`, followed by `12345` --- the final
    `12345` is not detected as a duplicate.

To detect duplicates across the entire input stream, the program would
need to retain all previously accepted card numbers, for example in a
COBOL table.

## Important Implementation Notes

-   `CARD-DUE` uses an implied decimal point (`V`). The decimal point is
    not stored as a character in the field.
-   The program uses interactive terminal input through `ACCEPT`; it
    does not currently read from an external sequential dataset.
-   `CARD-DETAILS-OUT` is a working-storage record used for displaying
    accepted data. The program does not write an output file.
-   The input and output counters represent accepted input entries and
    valid output records respectively.

## Build and Run

This is a COBOL source-code practice project. To compile and run it, use
a COBOL compiler such as GnuCOBOL or an IBM Enterprise COBOL
environment.

A typical GnuCOBOL command, after correcting and saving the source as
`CARDPGM01.cob`, is:

``` bash
cobc -x -free -o CARDPGM01 CARDPGM01.cob
./CARDPGM01
```

The `-free` option is appropriate only if the source is written in
free-format COBOL. If using fixed-format source, use the compiler's
fixed-format settings and place source lines accordingly.

## Source Review Status

The source shared with this README is a learning draft and should be
reviewed before compilation. In particular:

1.  The main processing loop must use valid
    `PERFORM UNTIL ... END-PERFORM` syntax.
2.  The paragraph name must be consistently spelled `1000-INITIAL-PARA`.
3.  The duplicate-check paragraph should have its own exit paragraph if
    it is invoked using `THRU`, for example `3110-EXIT`.
4.  Use standard straight single quotes (`'Y'`, `'N'`) in source code.
5.  Ensure the `DISPLAY` statement uses the declared counter name
    `WS-OUT-REC-COUNT`.

This README describes the intended behavior and structure; it does not
claim that the pasted source has been compiled or tested.

## Learning Concepts

This project is intended to practice:

-   COBOL program divisions and sections
-   Working-storage declarations and `VALUE` clauses
-   Group and elementary data items
-   `ACCEPT` and `DISPLAY`
-   `MOVE` and `ADD`
-   Conditional logic with `IF`, `ELSE`, and `END-IF`
-   Paragraphs and `PERFORM ... THRU`
-   Input/output counters
-   Basic validation and duplicate checking

## Future Enhancements

-   Read records from a sequential input file using `SELECT`, `FD`,
    `OPEN`, and `READ`.
-   Write accepted records to an output file using `WRITE`.
-   Add file-status handling.
-   Detect duplicates across all previously accepted records using a
    COBOL table.
-   Add a validation rule to reject a due amount greater than the card
    limit.
-   Separate invalid-record handling and reporting.

## Author

**Kartik**

`CARDPGM01` is a COBOL learning project focused on structured
paragraph-based processing and basic validation.
