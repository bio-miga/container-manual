# Assemble a Genome

If necessary, log into the HPCC with your username and password. Start MiGA:

```
cd
singularity shell MiGA
```


Create a directory for your project and move into it, and initialize a genome project:

```text
cd
mkdir $HOME/miga-data/miga_cap
cd $HOME/miga-data/miga_cap
miga new -P . -t genomes
```

Add your data set to the MiGA project. In doing this, turn off mytaxa scan and distances, as in the command below. MyTaxa is not installed in this version of MiGA, and because this project contains only one genome we cannot calculate distances. Here the -i flag specifies that the dataset being uploaded consists of trimmed reads.

```text
miga add -P . -t genome -i trimmed_reads_single  $HOME/miga-data/miga_genomes/a_capsulatum/A_capsulatum_reads.fasta -m run_mytaxa_scan=false,run_distances=false
```

For the input flag \(-i\), supported inputs include:

* raw\_reads\_single: Single raw reads in a single FastQ file
* raw\_reads\_paired: Paired raw reads in two FastQ files
* trimmed\_reads\_single: Single trimmed reads in a single FastA file
* trimmed\_reads\_paired: Paired trimmed reads in two FastA files
* trimmed\_reads\_interleaved: Paired trimmed reads in a single FastA file
* assembly: Assembled contigs or scaffolds in FastA format

Launch the daemon to start MiGA processing your data:

```text
miga daemon start -P . --shutdown-when-done
```

The `shutdown-when-done` argument automatically stops the daemon when processing is finished.

After the job starts, you can display the information about the job:

```text
miga about -P .
```

Also after the job starts, you can list the datasets being processed by entering:

```text
miga ls -P . -i
```

And you can monitor the job progress by entering:

```text
miga ls -P . -p
```

This should give something like:

```text
name                raw_reads  trimmed_reads  read_quality  trimmed_fasta  assembly  cds     essential_genes  ssu     mytaxa  mytaxa_scan  distances  taxonomy  stats
----                ---------  -------------  ------------  -------------  --------  ---     ---------------  ---     ------  -----------  ---------  --------  -----
A_capsulatum_reads  -          -              -             done           queued    queued  queued           queued  queued  queued       queued     queued    queued
```

Because the dataset submitted consisted of trimmed reads, trimmed\_fasta is the first column to have entries. When all entries under the stats column read "done," processing is finished.

If you did not use the "shutdown-when-done" argument when starting the daemon, you can stop it with the command:

```text
miga daemon stop -P .
```

Exit the MiGA singularity container with:

```text
exit
```

See the section "Exploring Results" for how to access the results of your project.

