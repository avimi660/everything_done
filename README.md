# everything_done
everything alana and I have done day by day except the sql solution
# note alanas code is here so go back and change to my own directories 

# 22/3/22
### This is how we ran it for the first gene in the file
gene_search_term=`head -n 2 mito_genes.txt | tail -n 1 | cut -f 4`
grep $gene_search_term /nesi/nobackup/uoo03398/alana_demo/data/GCF_011100635.1_mTriVul1.pri_genomic.gff | grep $'\t'gene$'\t' > temp
alt_search_terms=`head -n 2 mito_genes.txt | tail -n 1 | cut -f 5`
no_alts=`echo $alt_search_terms | grep -o ";" | wc -l`

for alt_gene in `seq 1 1 $no_alts`; do alt_gene_name=`echo $alt_search_terms | cut -d ";" -f $alt_gene | sed 's/ //g'`; grep $alt_gene_name /nesi/nobackup/uoo03398/alana_demo/data/GCF_011100635.1_mTriVul1.pri_genomic.gff | grep $'\t'gene$'\t' >> temp;
done

cat temp | sort | uniq > temp_all_searches 

no_matches=`wc -l temp_all_searches | awk '{ print $1 }'`

echo $gene_search_term $no_matches >> results_matches.txt

cat temp_all_searches > gene_location_possum.txt

rm temp*

#### Now we are going to run a loop for all the genes in mito_genes.txt

 no_mito_genes=`wc -l mito_genes.txt | awk '{ print $1 }'`
 for mito_line in `seq 2 1 $no_mito_genes`;
            do gene_search_term=`head -n $mito_line mito_genes.txt | tail -n 1 | cut -f 4`;
      grep "ID=gene-"$gene_search_term";" /nesi/nobackup/uoo03398/alana_demo/data/GCF_011100635.1_mTriVul1.pri_genomic.gff | grep $'\t'gene$'\t' > temp;
      alt_search_terms=`head -n $mito_line mito_genes.txt | tail -n 1 | cut -f 5`;
      no_alts=`echo $alt_search_terms | grep -o ";" | wc -l`;
                for alt_gene in `seq 1 1 $no_alts`;
do alt_gene_name=`echo $alt_search_terms | cut -d ";" -f $alt_gene | sed 's/ //g'`;
grep "ID=gene-"$alt_gene_name";" /nesi/nobackup/uoo03398/alana_demo/data/GCF_011100635.1_mTriVul1.pri_genomic.gff | grep $'\t'gene$'\t' >> temp;
done;

cat temp | sort | uniq > temp_all_searches;
no_matches=`wc -l temp_all_searches | awk '{ print $1 }'`;
echo $gene_search_term $no_matches >> results_matches.txt;
cat temp_all_searches >> gene_location_possum.txt;
rm temp*;
done

29/3/22: 
 # to install a package in R (only have to do this the first time)
library(tidyverse)
  results_table_tidyverse <- read_table("results_matches.txt")
# Reading in a table:
results_table_tidyverse <- read_delim("results_matches.txt", delim=" ", col_names=FALSE)
# Within tidyverse, select selects columns, filter filters rows
# This is a pipe that shoots one thing into another
%>%

multilple_gene_matches <- results_table_tidyverse %>% filter(X2>1)

zero_gene_matches <- results_table_tidyverse %>% filter(X2==0)

write_delim(multilple_gene_matches,"multiple_gene_matches.txt",delim=" ",col_names=FALSE)

write_delim(zero_gene_matches,"zero_gene_matches.txt",delim=" ",col_names=FALSE)

