[1]: https://rosettacode.org/wiki/Sorting_algorithms/Tree_sort_on_a_linked_list

# [Sorting algorithms/Tree sort on a linked list][1]

```perl
# 20231201 Raku programming solution

class BinaryTree { has ($.node, $.leftSubTree, $.rightSubTree) is rw;

   method insert($item) {
      if not $.node.defined {
         $.node = $item;
         ($.leftSubTree, $.rightSubTree)>>.&{ $_ = BinaryTree.new }
      } elsif $item cmp $.node < 0 {
         $.leftSubTree.insert($item);
      } else {
         $.rightSubTree.insert($item);
      }
   }

   method inOrder(@ll) {
      return unless $.node.defined;
      $.leftSubTree.inOrder(@ll);
      @ll.push($.node);
      $.rightSubTree.inOrder(@ll);
   }
}

sub treeSort(@ll) {
   my $searchTree = BinaryTree.new;
   for @ll -> $i { $searchTree.insert($i) }
   $searchTree.inOrder(my @ll2);
   return @ll2
}

sub printLinkedList(@ll, Str $fmt, Bool $sorted) {
   for @ll -> $i { printf "$fmt ", $i }
   $sorted ?? say() !! print "-> "
}

my @ll  = <5 3 7 9 1>;
#my @ll = [37, 88, 13, 18, 72, 77, 29, 93, 21, 97, 37, 42, 67, 22, 29, 2];
printLinkedList(@ll, "%d", False);
my @lls = treeSort(@ll);
printLinkedList(@lls, "%d", True);

my @ll2 = <d c e b a>;
#my @ll2 = <one two three four five six seven eight nine ten>;
printLinkedList(@ll2, "%s", False);
my @lls2 = treeSort(@ll2);
printLinkedList(@lls2, "%s", True);
```


You may [Attempt This Online!](https://ato.pxeger.com/run?1=fZTPbtNAEMYvnPwUX4NBieRaxAGSKG2KeuBUiUN6Qwg58Riv6qyj3XVDFOVJONADvFSfprP-U8cmxZK93pn9Zn4zu9pff1R4lz88_M1NfD55fPV7lYZa41rIUO1uFRH2SEKNvuvLLCIPrp9SbBb50jrtVIkfST0fQGio7cxxAKzJJFkEITUp03eFofUAe-vhR8SQmUEZ1Y8oFpKiZy8_pQeXKISzxtH_P8F87r_dw_3OyqYIX9IWhyrGAZRqzl8Exmq9qXNd4F2b4CiP3ypjdhyK2qJjmhdUTvFp9eiLikj1P6Vp0yJFJlcSuUyJd6TdqTpUl7EJU6_gf3-T66TawEGj7IB2pAeHCXW-hGH3IuMiGrj1Dq6mUK2S4oR0O13o40zZ3Difc6P5EB0Jmq4Myl60fSUI52B5UMJUrbCGGmujhDQ3Qt5RdCN0QedhYRTceG08XGdZynGZm6KKuktURIjRswL0PGusaAoVrq6gw11_gLOzci16LO1ZgBIOXPnFB4wwxhTD-cx5Xdkv8XU09jCZeBiO-OVxHPDLtmDqYcq2YMgjz-269-z7aH1B6Q--zZyT5fXeRMz5OeQzx30pk2nO1tqik1pdi29VbrWVOLAVRFiBsETYVFDYM0kw2wwmsZscZ7lCLO4JWvyEpnuSIHuCIIVdSHJ-MnNgM-t_sYMOd_AC-LO-JC_vqeq6qq-tJw)
