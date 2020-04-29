# Subset My BLAST
This simple script will generate a BLAST database alias of a subset of sequences based on a taxon tree. It requires a valid NCBI taxon ID or taxon name. The instructions below assume you are in a *nix OS (Linux or Mac).

## Dependencies
This script depends on:
1. TaxonKit: https://bioinf.shenwei.me/taxonkit/
2. BLAST+: ftp://ftp.ncbi.nlm.nih.gov/blast/executables/blast+/LATEST/
3. PIGZ: https://zlib.net/pigz/ 
4. CSVTK: https://github.com/shenwei356/csvtk

## Installation
Simply clone this repository, and make ssmb executable:
```bash
git clone https://github.com/jshleap/subset_my_blast.git
cd subset_my_blast
chmod +x ssmb
```

If you are an admin (if you have administrative credential, a.k.a sudo) copy the executable to your /usr/local/bin 
```bash
sudo cp ssmb /usr/local/bin
```

If you do not have SUDO priviledge include the path to the executable in your PATH variable:
```bash
export PATH=$PATH:$PWD
```

If you did the last step, this will have to be repeated at every login. If you want to make it permanent, add it to your bashrc or bash_profile:
```bash
echo "export PATH=$PATH:$PWD" >> ${HOME}/.bashrc
source ${HOME}/.bashrc
```

## Usage
You can get help by typing:
```bash
ssmb -h
```
You should get
```
Simple script to subset BLAST databases based on taxa. It requires taxonkit, csvtk, pigz, and blast+ installed
Usage: ssmb [-A|--acc2taxidfile <arg>] [--(no-)print] [-h|--help] <ID> <CPUS> <DB> <TYPE>
	<ID>: Taxid or valid NCBI taxon name
	<CPUS>: Number of cpus to use
	<DB>: PATH to the main database being subset
	<TYPE>: Whether is nucl or prot
	-A, --acc2taxidfile: Accession to taxid file. If not present will be downloaded (no default)
	--print, --no-print: boolean optional argument help msg (off by default)
	-h, --help: Prints help
```

### Example: Subset of the genus [Pimephales](https://en.wikipedia.org/wiki/Pimephales)
Say you have the nt database locally installed at `/home/databases/nt`. Turns out you only care about the fishes from the genus *Pimephales*, and you would like to create a subset database of said genus. `ssmb` has two options for this: with the taxid or with the taxon name. Both have to be valid for the NCBI. Let's say that you are running this in your laptop, and that it has 10 cpus available. Let's also say that you want nucleotides and **NOT** proteins, you can call ssmb, either like this:
```bash
ssmb Pimephales 10 /home/databases/nt nucl
```
or like this:
```bash
ssmb 51137 10 /home/databases/nt nucl
```
since *Pimephales*' NCBI tax ID is 51137.

In both of the above cases, ssmb assumes that you do not have the accession2taxid files downloaded (e.g. either nucl_gb.accession2taxid.gz for nucleotides or prot.accession2taxid.gz for proteins), so it will dowload it for you. If you already have these files, and would like to use your local copy, just pass the path to that file using the `-A` option. Let's asume that you have the `nucl_gb.accession2taxid.gz` in the `/home/accession2taxid` folder, then you can call `ssmb` as:
```bash
ssmb -A /home/accession2taxid/nucl_gb.accession2taxid.gz 51137 10 /home/databases/nt nucl
```

## Requests
This is a very basic script based on [Shenwei's](https://github.com/shenwei356) tutorials. If there is any other request, please submit an issue and, time permiting, I'll be happy to implement.
