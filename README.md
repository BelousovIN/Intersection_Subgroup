# Intersections $\pi$-Hall $D_\pi$-subgroups in finite simple sporadic groups


## Exhaustive search

The _OrbHall function implements an exhaustive enumeration algorithm for finding the number of all orbits under the action of the $\pi$-Hall subgroup $H$ on the set of all $\pi$-Hall subgroups conjugate to $H$ in the group $G$ that intersect $H$ in unity, where $G$ is a group isomorphic to the automorphism group of a simple sporadic group.


```gap
gap> OrbHall:=function(IdG, pi)
	local G,H,Cl_H, Intersect;
	
	if IdG in ["M11","M23","M24", "Co1", "Co2", "Co3", "Th", "Fi23", "Fi24", "B", "M", "J1",  "Ru", "J4", "Ly", "B", "M"] then
		G:=SimpleGroup(IdG);
	fi;
	
	if IdG in ["M12","M22",	"HS", "J2", "McL", "Suz", "He", "HN", "Fi22",  "ON", "J3"] then
		G:=AutomorphismGroup(SimpleGroup(IdG));
	fi;
	
	H:=HallSubgroup( G, pi );
	Cl_H:=ConjugateSubgroups(G,H);
	Intersect:=Filtered(Cl_H, x -> Order(Intersection(x,H))=1 );;
	return Size(Intersect)/Order(H);
end;
```

The OrbHall function for $\pi=${2} gives the answer for groups $M_{11}$, $M_{12}$, $M_{22}$, $M_{23}$, $M_{24}$, $HS$, $J_{2}$, $J_1$
```gap
gap> OrbHall("M11",[2]);
27
```

```gap
gap> OrbHall("M12",[2]);
6
```
```gap
gap> OrbHall("M22",[2]);
5
```
```gap
gap> OrbHall("M23",[2]);
522
```

```gap
gap> OrbHall("M24",[2]);
163
```
```gap
gap> OrbHall("HS",[2]);
39
```
```gap
gap> OrbHall("J2",[2]);
10
```

```gap
gap> OrbHall("J1",[2]);
127
```



The OrbHall function for $\pi=${2} does not give an answer for groups $Co_1$, $Co_2$, $Co_3$, $McL$, $Suz$, $He$, $HN$, $Th$,	$Fi_{22}$, $Fi_{23}$, $Fi_{24}'$, $B$, $M$, $O'N$, $J_3$, $Ru$, $J_4$, $Ly$. 


The OrbHall function for $\pi=${3} gives the answer for groups $M_{11}$, $M_{12}$, $M_{22}$, $M_{23}$, $M_{24}$, $HS$, $J_{2}$, $J_1$

```gap
gap> OrbHall("M11",[3]);
6
```

```gap
gap> OrbHall("M12",[3]);
31
```

```gap
gap> OrbHall("M22",[3]);
683
```
```gap
gap> OrbHall("M23",[3]);
7867
```

```gap
gap> OrbHall("M24",[3]);
41956
```
```gap
gap> OrbHall("HS",[3]);
17107
```
```gap
gap> OrbHall("J2",[3]);
102
```

```gap
gap> OrbHall("J1",[3]);
975
```


## Finding a Lower Bound for Orb_p(G) Using the Sylow $p$-Subgroup Center Centralizer

Let a group $G$ isomorphic to the automorphism group of a simple sporadic group, its Sylow $p$-subgroup $H$, and $C=C_G(Z(H))$. If there exists an element $g$ from a Sylow $q$-subgroup $A$ such that $C\cap H^g=1$, then $Orb_p(G)\ge |C:N_C(H^g)|/|H|$ .

The IntersectOrders function finds all the intersection orders of $C\cap H^g$, where $g$ runs through all elements of $A$, outputs an array of these orders, and if there exists an element $g$ with the above property, it takes the value $|C |/|N_G(H)|$. Otherwise, it takes the value $-1$.  

```gap
gap> IntersectOrders:=function(IdG, p, q)
	local G,H, A, C, orbH, ListOrderIntersectH;
	
	if IdG in ["M11","M23","M24", "Co1", "Co2", "Co3", "Th", "Fi23", "Fi24", "B", "M", "J1",  "Ru", "J4", "Ly", "B", "M"] then
		G:=AtlasGroup(IdG);
	fi;
	
	if IdG in ["M12","M22",	"HS", "J2", "McL", "Suz", "He", "HN", "Fi22",  "ON", "J3"] then
		G:=AtlasGroup(Concatenation(IdG,".2"));
	fi;
	
	H:=SylowSubgroup(G,p);
	A:=SylowSubgroup(G,q);
	C:=Centralizer(G,Center(H));
	orbH:=H^A;
	ListOrderIntersectH:=List(orbH, x-> Order(Intersection(x,C)));
	if 1 in  ListOrderIntersectH then 
		Print(ListOrderIntersectH, "\n");
		return Order(C)/(Order(Normalizer(C,orbH[Position(ListOrderIntersectH,1)]))*Order(H));
	else
		Print(ListOrderIntersectH, "\n");
		return -1;
	fi;
	
end;
```

$p=2$

```gap
gap> IntersectOrders("M11",2,11);
[ 16, 2, 1, 2, 1, 1, 1, 1, 2, 1, 2 ]
3
```

```gap
gap> IntersectOrders("M12",2,11);
[ 128, 4, 1, 2, 2, 2, 4, 2, 2, 2, 2 ]
3

```

```gap
gap> IntersectOrders("M22",2,11);
[ 256, 2, 8, 4, 2, 1, 1, 4, 4, 16, 2 ]
3
```

```gap
gap> IntersectOrders("M23",2,23);
[ 128, 1, 1, 1, 1, 16, 8, 1, 1, 1, 8, 1, 1, 1, 1, 1, 1, 1, 4, 1, 1, 1, 2 ]
21


```

```gap
gap> IntersectOrders("M24",2,23);
[ 1024, 2, 1, 2, 1, 2, 1, 1, 2, 1, 32, 4, 2, 32, 4, 8, 1, 1, 1, 4, 2, 4, 4 ]
21
```

```gap
gap> IntersectOrders("HS",2,11);
[ 1024, 2, 2, 4, 4, 2, 2, 2, 2, 2, 2 ]
-1
```

```gap
gap> IntersectOrders("J2",2,7);
[ 256, 8, 2, 2, 2, 2, 8 ]
-1
```

```gap
gap> IntersectOrders("Co1",2,23);
[ 2097152, 4, 1, 4, 32, 2, 2, 1, 32, 16, 2, 2, 8, 1, 2, 128, 4, 4, 4, 32, 4, 1, 4 ]
42525
```

```gap
gap> IntersectOrders("Co2",2,23);
[ 262144, 32, 32, 64, 128, 64, 32, 32, 256, 64, 64, 32, 32, 16, 256, 64, 64, 256, 32, 64, 64, 16, 32 ]
-1
```

```gap
gap> IntersectOrders("Co2",2,11);
[ 262144, 64, 128, 64, 32, 32, 16, 16, 64, 32, 64 ]
-1
```

```gap
gap> IntersectOrders("Co2",2,7);
[ 262144, 64, 32, 64, 128, 64, 16 ]
-1
```

```gap
gap> IntersectOrders("Co3",2,23);
[ 1024, 1, 1, 1, 2, 1, 1, 1, 2, 2, 1, 4, 2, 1, 2, 2, 2, 1, 1, 1, 1, 1, 2 ]
2835
```

```gap
gap> IntersectOrders("McL",2,11);
[ 256, 2, 2, 2, 1, 2, 1, 1, 2, 1, 1 ]
315
```

```gap
gap> IntersectOrders("Suz",2,13);
[ 16384, 1, 8, 1, 1, 4, 1, 2, 16, 2, 1, 8, 2 ]
405
```

```gap
gap> IntersectOrders("He",2,17);
[ 2048, 1, 1, 4, 1, 2, 1, 4, 1, 1, 2, 1, 1, 2, 8, 1, 1 ]
21
```

```gap
gap> IntersectOrders("HN",2,19);
[ 32768, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1 ]
225
```
```gap
gap> IntersectOrders("Fi22",2,13);
[ 262144, 8, 8, 8, 8, 4, 32, 16, 8, 4, 8, 8, 16 ]
-1
gap> IntersectOrders("Fi22",2,11);
[ 262144, 16, 8, 4, 8, 8, 8, 8, 8, 4, 2 ]
-1
gap> IntersectOrders("Fi22",2,7);
[ 262144, 8, 8, 64, 128, 2, 8 ]
-1
```
```gap
gap> IntersectOrders("Fi23",2,17);
[ 262144, 1, 2, 1, 2, 1, 2, 1, 1, 1, 1, 2, 1, 4, 1, 1, 1 ]
405
```

```gap
gap> IntersectOrders("Fi24",2,17);
[ 4194304, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2, 1, 1, 1 ]
76545
```

```gap
gap> IntersectOrders("J1",2,19);
[ 8, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1 ]
1
gap> IntersectOrders("J1",2,11);
[ 8, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1 ]
1
gap> IntersectOrders("J1",2,7);
[ 8, 1, 1, 1, 1, 1, 1 ]
1
```

```gap
gap> IntersectOrders("ON",2,31);
[ 1024, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1 ]
315
```
```gap
gap> IntersectOrders("J3",2,19);
[ 256, 1, 1, 1, 1, 1, 1, 1, 1, 2, 1, 1, 1, 1, 1, 1, 1, 1, 1 ]
15
```
```gap
gap> IntersectOrders("Ru",2,13);
[ 16384, 2, 2, 1, 1, 2, 2, 1, 2, 1, 1, 2, 1 ]
15
```

$p=3$

```gap
gap> IntersectOrders("M11",3,11);
[ 9, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1 ]
1
```

```gap
gap> IntersectOrders("M12",3,11);
[ 27, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1 ]
4
```

```gap
gap> IntersectOrders("M22",3,11);
[ 9, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1 ]
1
```

```gap
gap> IntersectOrders("M23",3,11);
[ 9, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1 ]
1
```

```gap
gap> IntersectOrders("M24",3,11);
[ 27, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1 ]
20
```

```gap
gap> IntersectOrders("HS",3,11);
[ 9, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1 ]
4
```

```gap
gap> IntersectOrders("J2",3,7);
[ 27, 1, 1, 1, 1, 1, 1 ]
10
```

```gap
gap> IntersectOrders("Co3",3,23);
[ 2187, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 3, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1 ]
160
```
```gap
gap> IntersectOrders("Co2",3,23);
[ 729, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1 ]
640
```

```gap
gap> IntersectOrders("McL",3,11);
[ 729, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1 ]
80
```

```gap
gap> IntersectOrders("Suz",3,13);
[ 2187, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1 ]
16
```

```gap
gap> IntersectOrders("He",3,17);
[ 27, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1 ]
560

```


```gap
gap>  IntersectOrders("Fi23",3,23);
[ 1594323, 9, 1, 1, 1, 1, 3, 3, 1, 3, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 81 ]
1024
```
```gap
gap> IntersectOrders("HN",3,19);
[ 729, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1 ]
80

```

```gap
gap> IntersectOrders("Fi22",3,13);
[ 19683, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1 ]
256
```
```gap
gap> IntersectOrders("Fi23",3,13);
[ 1594323, 1, 1, 3, 1, 1, 1, 1, 1, 1, 1, 1, 1 ]
1024
```
```gap
gap> IntersectOrders("Fi24",3,13);
[ 43046721, 1, 1, 3, 1, 1, 1, 1, 1, 1, 1, 3, 1 ]
56320
```
```gap
gap> IntersectOrders("J1",3,19);
[ 3, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1 ]
10
```

```gap
gap> IntersectOrders("ON",3,31);
[ 81, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1 ]
1
```

```gap
gap> IntersectOrders("J3",3,19);
[ 243, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1 ]
1
gap> IntersectOrders("J3",3,17);
[ 243, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1 ]
1
gap> IntersectOrders("J3",3,5);
[ 243, 1, 1, 1, 1 ]
1
```

```gap
gap> IntersectOrders("Ru",3,29);
[ 27, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1 ]
80
```

## Нахождение нижней границы для количества орбит, используя силовскую подгруппу большого простого порядка.

```gap
gap> G:=AtlasGroup("J3");;
gap> H:=SylowSubgroup(G,3);;
gap> A:=SylowSubgroup(G,19);;
gap> orbH:=H^A;;
gap> Intersect1:=Filtered(orbH,x->Order(Intersection(H,x))=1);
[ <permutation group of size 243 with 4 generators>, <permutation group of size 243 with 4 generators>, <permutation group of size 243 with 4 generators>,
  <permutation group of size 243 with 4 generators>, <permutation group of size 243 with 4 generators>, <permutation group of size 243 with 4 generators>,
  <permutation group of size 243 with 4 generators>, <permutation group of size 243 with 4 generators>, <permutation group of size 243 with 4 generators>,
  <permutation group of size 243 with 4 generators>, <permutation group of size 243 with 4 generators>, <permutation group of size 243 with 4 generators>,
  <permutation group of size 243 with 4 generators>, <permutation group of size 243 with 4 generators>, <permutation group of size 243 with 4 generators>,
  <permutation group of size 243 with 4 generators>, <permutation group of size 243 with 4 generators>, <permutation group of size 243 with 4 generators> ]


```

```gap
gap> Orbit1:=Intersect1[1]^H;;
gap> Intersect1[2] in Orbit1;
false
```
