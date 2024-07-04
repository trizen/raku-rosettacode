[1]: https://rosettacode.org/wiki/Sealed_classes_and_methods

# [Sealed classes and methods][1]

Raku doesn't have final class but [Roles](https://docs.raku.org/syntax/role) is normally used to mimic the goal.

```perl
# 20240626 Raku programming solution

role MovieWatcherRole { has Str $.name;
   method WatchMovie() { say "$.name is watching the movie"   }
   method EatPopcorn() { say "$.name is enjoying the popcorn" }
}

class MovieWatcher does MovieWatcherRole {
   method new(Str $name) { self.bless(:$name) }
}

class ParentMovieWatcher is MovieWatcher {
   method new(Str $name) { self.bless(:$name) }
}

role ChildMovieWatcherRole {
   method EatPopcorn() { say "$.name is eating too much popcorn" }
}

class ChildMovieWatcher is MovieWatcher does ChildMovieWatcherRole {
   method new(Str $name) { self.bless(:$name) }
}

role YoungChildMovieWatcherRole {
   method WatchMovie() { 
      say "Sorry, $.name, you are too young to watch the movie."; 
   }
}

class YoungChildMovieWatcher is ChildMovieWatcher does YoungChildMovieWatcherRole {
   method new(Str $name) { self.bless(:$name) }
}

for ParentMovieWatcher.new('Donald'),
    ChildMovieWatcher.new('Lisa'),
    YoungChildMovieWatcher.new('Fred')
{ .WatchMovie and .EatPopcorn }
```


You may [Attempt This Online!](https://ato.pxeger.com/run?1=nVPBToNAED144ysmxKRtgpyNPVo9adLYg_G4wlBQ2Gl2FxvS8CVeetCf8uKvuLtAoIVIldNm9r15bx6z7x-Cveb7_WeuoovLr7NvQSnCPb0l-MhUEKN4MIUdxEzCSgk49znLcO4AQIYqphAszjKmMw2UrAC3QkEiYWtuE74GFSNkBuVqatnh3zC1pE1Agg_xkb9Q0fA3Fc7V_NJxgpRJeeAVQkI54L6jxnE7tXMYAauHaeQ_pyjl9KoudrovmUCuDjSSI81_dbcxX8dJGv7qdiQbpmwyRJDlQTwYT0-j599mNm7lb6M9Uc7X402Pdsfc6M-OuSIhCq_eNw8KykH_CztsYZrrU7Vc7Wb57ty26Iw_7MNk0C_aIE40fnIaEYmBJfINf7IgztJwMvPs3D3RCnSXSNZAhs1VuFuBupWzA78NFRgPwW-XCMrqndfPvXn2Pw)
