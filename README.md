# Overview

This project aims at providing the structure to organise small project with Excel. The goal is to separate the files that defines the keywords and what each experiments correspond to, from the data/experiments themselves. The excel files will keep track of the metadata and real data/statistics that define the data in the folder they are into. Then, all the information are aggregated in one file to have an overview of every elements.

Every protocols, data, keywords, elements that define a data and its derivatives are defined in the main folder. Then every data keeps track of what happens in the main folder so that everything is in **sync and share the same vocabulary**. This make sharing individual folders of experiments/derivatives easy as they keep a copy of the parameters that were defined in the main folder.

Careful: you still need to manually create the folders/data, make them keeping track of the templates and manually add them 

> What is a "**small project**"? Any project where you could feel that you would be able to handle the **creation** and **updates** of your files manually. Excel can handle a limited amount of rows and files but if you ever reach its limits, it probably means that should not have used Excel anyway :).

> What is a "**derivative**"? A derivative is anything that derives from an original data; it can be seen as any analysis or data generated that are based on the raw data. A raw data could be a client and she/he bought in a shop. Then a derivative could be a subset of the raw data that only take into account what products bought are organic.

***

# Files and folder organisation

The project is organised around two levels: a main folder and experiments. The exact folder names can be adapted to the project.

```javascript
/Main
│
├── template/
│   ├── schema/
│   ├── keywords/
│   └── protocols/
│
└── join/
/Experiment1
│
├── raw_data/
│   └── raw.xlsx
│
├── derivative1/
│   └── derivative1.xlsx
│
└── derivative2/
    └── derivative2.xlsx
/Experiment2
│
├── ...
```

The `Main` folder contains the definitions shared by the project.

- **schema:&#32;**defines the fields. It describes what information exists, but does not contain the actual experimental data.
- **keywords:** defines the allowed vocabulary in schema.
- **protocol:** explains how the data were generated or processed. It can document, for example: how an experiment was performed, how raw data were acquired, how a derivative was generated, what processing steps were applied, or any other information needed to understand how the data were produced.

The `join` files aggregate the individual datasets.

```javascript
Experiment1 ─┐
Experiment2 ─┤
Experiment3 ─┼──→ Power Query → Join
Experiment4 ─┘
```

Each `experiment` or dataset has its own folder and keeps local copies of the definitions relevant to that dataset

***

# How it work under the hood: syncing, creation, delete

To keep track of the different files, updates and to summarize the different datasets, these set of excel files only rely on PowerQuery and Pivot Tables; with both tools, syncinc is done automatically in the toolbar of Excel. In main/template/schema and main/template/keywords, parameters can simply be added one after another in the column. To keep things organize, it is best to always create a new spreadsheet when a new experiment/dataset must be created. The same must be done with the protocols.

The syncing, creation concers mostly the experiment files and the main/join that aggregate every dataset:

In the `join` file:

- **Update import**: join-raw/derivative: go to PowerQuery -> open editor. On the left side panel, right click and add the new excel file that correspond to the new excel file that correspond to the new raw/derivative. Then, within the Join table add the added raw/derivative as new column; to properly do this you need to do it before the table is transpose.

Careful when doing the import as Excel can sometimes considerate that a field has a different type than the one it has in the experimental file. For example a date in an experiment might seem to appear as a number in the join file; this is just excel switching between the types (a date can be represented as a number), so you can just force excel to consider it as a date.
- **Creation subresearch**: to create a subresearch via a pivot table, right click on the join-raw/derivative table and Pivot table. Now, select only the fields you are interested in and place them as row. You can add a filter to only see data that respect an element by adding a field in Filter.

In each `experiment` or dataset:

- **Creation**: after creating a folder for you new data, create and save an excel file within the folder you want to keep track of the metadata/data. Then go to PowerQuery -> import the corresponding schema, keyword and protocol from the main folder.
- **Writing and updates**: within the files, **/!\\ do not write within the column that correspond to the Schema table /!\\** ; this is because if the main/schema file is updated, the data will vanish after the syncing in the raw/experiment file. While if they are kept in another column, there might be a displacement (if a line in main/schema was removed for example) but at least the data are still there. 

To use the defined keywords, go to data->data validation->list-> select the column with the list of keywords.
- **delete**: simply remove the folder and subsequent excel file. Do not forget to update the main/join file if certain elements were synced.

