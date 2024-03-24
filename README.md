# geo-php-native-solution
geo php native solution

```php
<?php
function doLinesCross($line1, $line2)
{
    list($x1, $y1) = $line1[0];
    list($x2, $y2) = $line1[1];
    list($x3, $y3) = $line2[0];
    list($x4, $y4) = $line2[1];

    $d = ($x1 - $x2) * ($y3 - $y4) - ($y1 - $y2) * ($x3 - $x4);
    if ($d == 0) {
        return false; // Lines are parallel
    }

    $ua = (($x4 - $x3) * ($y1 - $y3) - ($y4 - $y3) * ($x1 - $x3)) / $d;
    $ub = (($x2 - $x1) * ($y1 - $y3) - ($y2 - $y1) * ($x1 - $x3)) / $d;

    return ($ua >= 0 && $ua <= 1 && $ub >= 0 && $ub <= 1);
}

//farm 1
$firstMapData = [
    [3.374501343736007, 6.531343195836656],
    [3.374533530244186, 6.530160022843568],
    [3.375150438317611, 6.530298592798164],
    [3.375155802735641, 6.531417810255979]
];

//farm 2
$secondMapData = [
    [3.374288756457946, 6.531482285932473],
    [3.3743316718021843, 6.530720152509834],
    [3.375061232654235, 6.530165872928058],
    [3.3747715540806267, 6.531604866724261]
];

//crosess
$crosses = [
    'status' => [],
    'cross_data' => []
];
// Check if any pair of lines cross each other
for ($i = 0; $i < count($firstMapData) - 1; $i++) {
    for ($j = 0; $j < count($secondMapData) - 1; $j++) {
        $line1 = [$firstMapData[$i], $firstMapData[$i + 1]];
        $line2 = [$secondMapData[$j], $secondMapData[$j + 1]];
        if (doLinesCross($line1, $line2)) {
            $crosses['status'][] = true;
            //return where it crosses the line and the index
            $crosses['cross_data'][] = [
                'cross_point' => [
                    'line1' => $line1,
                    'line2' => $line2
                ],
                'cross_index' => [$i, $j]
            ];
        }
    }
}

//check if the crosses has status true
if (in_array(true, $crosses['status'])) {
    echo json_encode($crosses);
} else {
    echo json_encode(['status' => false]);
}


```
