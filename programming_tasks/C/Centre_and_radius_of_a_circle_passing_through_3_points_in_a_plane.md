[1]: https://rosettacode.org/wiki/Centre_and_radius_of_a_circle_passing_through_3_points_in_a_plane

# [Centre and radius of a circle passing through 3 points in a plane][1]

Don't bother defining all the intermediate variables.

```perl
sub circle( (\𝒳ᵢ, \𝒴ᵢ), (\𝒳ⱼ, \𝒴ⱼ), (\𝒳ₖ, \𝒴ₖ) ) {
    :center(
        my $𝒞ₓ = ((𝒳ᵢ² + 𝒴ᵢ²) × (𝒴ₖ - 𝒴ⱼ) + (𝒳ⱼ² + 𝒴ⱼ²) × (𝒴ᵢ - 𝒴ₖ) + (𝒳ₖ² + 𝒴ₖ²) × (𝒴ⱼ - 𝒴ᵢ)) /
                  (𝒳ᵢ × (𝒴ₖ - 𝒴ⱼ) + 𝒳ⱼ × (𝒴ᵢ - 𝒴ₖ) + 𝒳ₖ × (𝒴ⱼ - 𝒴ᵢ)) / 2,
        my $𝒞ᵧ = ((𝒳ᵢ² + 𝒴ᵢ²) × (𝒳ₖ - 𝒳ⱼ) + (𝒳ⱼ² + 𝒴ⱼ²) × (𝒳ᵢ - 𝒳ₖ) + (𝒳ₖ² + 𝒴ₖ²) × (𝒳ⱼ - 𝒳ᵢ)) /
                  (𝒴ᵢ × (𝒳ₖ - 𝒳ⱼ) + 𝒴ⱼ × (𝒳ᵢ - 𝒳ₖ) + 𝒴ₖ × (𝒳ⱼ - 𝒳ᵢ)) / 2
    ),
    radius => (($𝒞ₓ - 𝒳ᵢ)² + ($𝒞ᵧ - 𝒴ᵢ)²).sqrt
}

say circle (22.83,2.07), (14.39,30.24), (33.65,17.31);
```

#### Output:
```
(center => (18.97851566 16.2654108) radius => 14.70862397833418)
```


You may [Attempt This Online!](https://ato.pxeger.com/run?1=jZO9TsMwFIUHtjzFHRhs4ZrGLrSAypOwhBCkih8JJxkqxJJHACkMCFQhEHPrdutU3oKtfQIeATv1T6UqKJ6ur3x8vntkv3yI6CofjT7z7LLV-9khaX4O8UDE1wkCdPb7-iiXs3cCupqqChPTXY3npqsq3y1K2y1KDBjuA1DrOE5us0SgaqPXzRB21am3VfEEfUDI-CwmsAfGaTHB8P0MyFwGLbBm6gwyCE5Q1V6g9FagOaygKL1A1xsO47kR6CEx7DtUvyxlPZahqscwFPW2wMhWSMvZV4OQpKORzUKSjk42C0k6Wvl_SNONkLax7Ny1GDbaWltglTFeRyWii0GeQv9UReQelT9fDYNckD5tNRlN70QWPARBGg3NqwfEGO1xwmi7q5912KH8iPA2ZR295ZweHpCwS3mIT9afxvwd-4f-AA)
