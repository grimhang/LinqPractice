---
sort: 13
comments: true
---

# 조인 연산자 : Join, GroupJoin, Left Join, Cross Join
SQL Server, Oracle, MySQL 등과 같은 데이터베이스를 사용해 봤다면 SQL의 Join에 익숙할 것이다. LINQ 조인도 동일하게 두 개 이상의 데이터 원본(테이블 또는 개체)에서 공통 속성(컬럼)을 이용하여 1개의 결과로 합치는 것이다. 


예. 다음 두 가지 데이터 소스 Album(앨범)과  세부곡(Track)이 있을때
![13_01_Sample1.png](image/13/13_01_NewSample1.png)  


공통 속성인 AlbumId를 이용하여 조인된 결과는 아래와 같다.  
![13_02_Sample2.png](image/13/13_02_NewSample2.png)  

**조인의 종류**  

1. Join: 공통 속성을 기반으로 두 데이터 소스 또는 컬렉션을 결합하고 단일 결과 집합으로 만드는 데 사용된다.
2. GroupJoin: 공통 속성을 기반으로 두 데이터 소스 또는 컬렉션을 결합하는 데도 사용되지만 결과를 시퀀스 그룹으로 반환.

그 중 Join은 다음과 같은 여러가지 종류가 있다.

![13_03_JoinType.png](image/13/13_03_JoinType.png)  

**샘플데이터**  

이번에는 4개의 샘플 컬렉션 데이터 Artist, Album, Track,  PlayList 를 사용할 것이다.

```cs
using System.Collections.Generic;

namespace LINQJoin
{
    public class Artist
    {
        public int ArtistId { get; set; }
        public string Name { get; set; }

        public static List<Artist> GetAllArtists()
        {
            return new List<Artist>
            {
                new Artist {ArtistId = 1, Name = "AC/DC"},
                new Artist {ArtistId = 2, Name = "Accept"},
                new Artist {ArtistId = 3, Name = "Aerosmith"},
            };
        }
    }

    public class Album
    {
        public int AlbumId { get; set; }
        public string Title { get; set; }

        public static List<Album> GetAllAlbums()
        {
            return new List<Album>()
            {
                new Album { AlbumId = 1, Title = "For Those About To Rock", ArtistId = 1},
                new Album { AlbumId = 2, Title = "Balls to the Wall", ArtistId = 2},
                new Album { AlbumId = 3, Title = "Restless and Wild", ArtistId = 2},
                new Album { AlbumId = 4, Title = "Let There Be Rock", ArtistId = 1},
                new Album { AlbumId = 5, Title = "Big Ones", ArtistId = 3 },
                new Album { AlbumId = 6, Title = "Jagged Little Pill", ArtistId = 4 },
            };
        }
    }

    public class Track
    {
        public int AlbumId { get; set; }
        public int TrackNo { get; set; }
        public string Name { get; set; }            

        public static List<Track> GetAllTracks()
        {
            return new List<Track>()
            {
                new Track { AlbumId = 1, TrackNo = 1, Name = "For Those About To Rock (We Salute You)"},
                new Track { AlbumId = 1, TrackNo = 2, Name = "Put The Finger On You"},
                new Track { AlbumId = 1, TrackNo = 3, Name = "Let's Get It Up",},
                new Track { AlbumId = 1, TrackNo = 4, Name = "Inject The Venom"},
                new Track { AlbumId = 1, TrackNo = 5, Name = "Snowballed"},
                new Track { AlbumId = 1, TrackNo = 6, Name = "Breaking The Rules"},
                new Track { AlbumId = 2, TrackNo = 1, Name = "Balls to the Wall"},
                new Track { AlbumId = 3, TrackNo = 1, Name = "Fast As a Shark"},
                new Track { AlbumId = 3, TrackNo = 2, Name = "Restless and Wild"},
                new Track { AlbumId = 3, TrackNo = 3, Name = "Princess of the Dawn"},
            };
        }
    }

    public class PlayList
    {            
        public int AlbumId { get; set; }
        public int TrackNo { get; set; }            
        public int CustomerId { get; set; }

        public static List<PlayList> GetAllPlayLists()
        {
            return new List<PlayList>()
            {
                new PlayList { AlbumId = 1, TrackNo = 1, CustomerId = 1},
                new PlayList { AlbumId = 1, TrackNo = 2, CustomerId = 1},
                new PlayList { AlbumId = 1, TrackNo = 3, CustomerId = 1},
                new PlayList { AlbumId = 3, TrackNo = 1, CustomerId = 1},
            };
        }
    }    
}
```

데이터들의 관계를 ERD로 표현하면 다음과 같다.
![13_00_SampleDataErd.png](image/13/13_00_SampleDataErd.png)

<br/>

## <font color='dodgerblue' size="6">1) Inner Join</font>     

- ### A. Inner Join
    두 데이터 소스에서 일치하는 요소만 결과에 포함한다. 

    ![13_04_InnerJoin.png](image/13/13_04_InnerJoin.png)  

    내부 조인을 수행하는 동안 두 데이터 원본에 공통 요소 또는 속성이 있어야 합니다.

    이 연산자는 두 컬렉션의 데이터를 합쳐 새 컬렉션을 반환하며 SQL 조인과 동일하다.  
    
    잠깐 Linq Join 메서드의 정의를 살펴보자.

    ![13_05_JoinMethod.png](image/13/13_05_JoinMethod.png)  

    웁쓰. 이해하기 너무 복잡하다. 한가지만 기억하면 되는데 두 가지 오버로드된 버전이 존재하고 차이점은 두 번째 오버로드된 버전에서는 비교자를 추가 매개변수로 받아들인다는 것이다.

    Linq Join은 다음 5가지 사항을 이해해야 한다.

        1. 외부 데이터 소스
        2. 내부 데이터 소스
        3. 외부 키 선택기(외부 데이터 소스 중 조인에 사용될 공통 키)
        4. 내부 키 선택기(내부 데이터 소스 중 조인에 사용될 공통 키)
        5. 결과 선택기(최종 결과에서 원하는 컬럼만 뽑아내기. 즉 Select절)


<br/>

- ### B.메서드 또는 쿼리 구문을 사용하는 Inner Join 예제

        1. 간단 예제(2개 테이블 조인)
        2. 복잡 예제(데이터 소스가 2개보다 많을때)
        3. 복잡 예제(조인 컬럼이 2개보다 많을때)



    **예제1: 간단예제(2개테이블 조인)**  
    다음은 앨범과 트랙정보를 Join하여 앨범타이틀, 트랙제목 2가지만을 보이게 할 것이다. 양쪽에 공통으로 존재하는 속성인 AlbumId를 이용해 조인을 수행한 후 new 익명형식으로 결과를 만든다.  
    
    ```cs
    using System.Linq;
    using System;

    namespace LINQJoin
    {
        class Program
        {
            static void Main(string[] args)
            {
                //Linq Method
                var JoinMS = Album.GetAllAlbums()               // 외부테이블. 드라이빙 테이블이라고도함
                    .Join(
                            Track.GetAllTracks(),               // 내부테이블
                            album => album.AlbumId,             // 외부테이블의 조인키
                            tr => tr.AlbumId,                   // 내부테이블의 조인키
                            (album, tr) => new                  // 조인 후 원하는 컬럼만 추출
                            {
                                AlbumTitle = album.Title,
                                TrackName = tr.Name,
                            }
                        );

                //Linq Query
                var JoinQS = (from album in Album.GetAllAlbums()
                          join tr in Track.GetAllTracks()
                            on album.AlbumId equals tr.AlbumId      // 조인 컬럼. 주의:on바로 뒤에는 첫번째 테이블
                          select new
                          {
                              AlbumTitle = album.Title,
                              TrackName = tr.Name,
                          }).ToList();

                foreach (var track in JoinMS)
                {
                    Console.WriteLine($"Album: {track.AlbumTitle, -25}, TrackTitle: {track.TrackName}");
                }

                Console.ReadKey();
            }
        }
    }
    ```

    결과  
    ![13_06_JoinExam1Result.png](image/13/13_06_NewJoinExam1Result.png)  
    
    두 데이터 소스에서 일치하는 레코드만 가져온다. sql에 익숙하다면 싶게 이해할수 있을 것이다.
  
    <br>
    **예제2: 복잡 예제(데이터 소스가 2개보다 많을때)**  
    데이터가 3개 이상일 경우 Join을 1번 더 사용해야 한다.
    
    다음은 아티스트명, 앨범명, 트랙제목 3개 속성만 추출해보자.

    Artist와 Album의 공통속성은 ArtistId,   
    Album과 Track의 공통속성은 AlbumId이다.    
    
    쉬운 버전
    ```cs
    using System.Linq;
    using System;

    namespace LINQJoin
    {
        class Program
        {
            static void Main(string[] args)
            {
                //Linq Method
                var JoinMS = Album.GetAllAlbums()           
                    .Join(
                            Artist.GetAllArtists(),         
                            album => album.ArtistId,        
                            art => art.ArtistId,            
                            (album, art) => new             
                            {
                                AlbumId = album.AlbumId,
                                ArtistName = art.Name,
                                AlbumTitle = album.Title
                            }
                        )
                    .Join(
                        Track.GetAllTracks(),
                        albumArt => albumArt.AlbumId,
                        tr => tr.AlbumId,
                        (albumArt, tr) => new
                        {
                            Artistname = albumArt.ArtistName,
                            AlbumTitle = albumArt.AlbumTitle,
                            TrackName = tr.Name
                        }
                    ).ToList();

                //Linq Query
                var JoinQS = (
                                from album in Album.GetAllAlbums()          // 첫번째 테이블
                                join art in Artist.GetAllArtists()          // 두번째 테이블
                                    on album.ArtistId equals art.ArtistId   // 조인컬럼.주의:on바로 뒤에는 첫번째 테이블의 키 
                                join tr in Track.GetAllTracks()
                                    on album.AlbumId equals tr.AlbumId
                                select new
                                {
                                    Artistname = art.Name,
                                    AlbumTitle = album.Title,
                                    TrackName = tr.Name
                                }
                            ).ToList();  

                foreach (var item in JoinMS)
                {
                    Console.WriteLine($"Artist: {item.Artistname, -10}, Album: {item.AlbumTitle,-25}, " +
                        $"TrackTitle: {item.TrackName}");
                }

                Console.ReadKey();
            }
        }
    }
    ```

    결과  
    ![13_07_NewJoinExam2Result.png](image/13/13_07_NewJoinExam2Result.png)  
    
    각 단계별로 필요한 컬럼만 추출하여 다음단계와 비교한다. 이해하기 쉽다.


    어려운 버전: 몰라도 됨
    ```cs
    using System.Linq;
    using System;

    namespace LINQJoin
    {
        class Program
        {
            static void Main(string[] args)
            {
                //Linq Method
                var JoinMS = Album.GetAllAlbums()
                    .Join(
                            Artist.GetAllArtists(),
                            album => album.ArtistId,
                            ar => ar.ArtistId,
                            (album, ar) => new
                            {
                                ar, album
                            }
                        )
                    .Join(
                        Track.GetAllTracks(),
                        albumArtist => albumArtist.album.AlbumId,
                        tr => tr.AlbumId,
                        (albumArtist, tr) => new
                        {
                            tr, albumArtist
                        }
                    ).ToList();

                foreach (var item in JoinMS)
                {
                    Console.WriteLine($"Artist: {item.albumArtist.ar.Name, -10}, Album: {item.albumArtist.album.Title,-25}, " +
                        $"TrackTitle: {item.tr.Name}");
                }

                Console.ReadKey();
            }
        }
    }
    ```
    각 단계별로 필요한 컬럼만 추출하지 않고 모든 컬럼을 추출. 이해가 어렵다.
    

    <br>
    **예제3: 복잡 예제(조인 컬럼이 2개보다 많을때)**  
    테이블의 PK가 2개이상일 경우처럼 조인에 사용될 속성이 2개이상인 경우도 자주 발생한다. 그럴때는 조인컬럼을 위한 new 익명형식을 만들어서 비교해야 한다.  
    다음은 플레이리스트를 추출할때 각 트랙의 제목까지 추출한 예제이다.

    PlayList와 Track의 공통속성은 AlbumId, TrackNo이다.    
    
    ```cs
    using System.Linq;
    using System;

    namespace LINQJoin
    {
        class Program
        {
            static void Main(string[] args)
            {
                //Linq Method
                var JoinMS = PlayList.GetAllPlayLists().Join(
                Track.GetAllTracks(),
                pl => new { pl.AlbumId, pl.TrackNo },
                tr => new {tr.AlbumId, tr.TrackNo},
                (pl, tr) => new
                {
                    CustomerId = pl.CustomerId,
                    AlubmId = tr.AlbumId,
                    TrackNo = tr.TrackNo,
                    TrackName = tr.Name,
                });

                //Linq Query
                var JoinQS = (from pl in PlayList.GetAllPlayLists()
                            join tr in Track.GetAllTracks()
                                on new { pl.AlbumId, pl.TrackNo } equals new { tr.AlbumId, tr.TrackNo }
                            select new
                            {
                                CustomerId = pl.CustomerId,
                                AlubmId = tr.AlbumId,
                                TrackNo = tr.TrackNo,
                                TrackName = tr.Name,
                            }).ToList();

                foreach (var pl in JoinMS)
                {
                    Console.WriteLine($"CustomerId: {pl.CustomerId}, AlbumId:{pl.AlubmId}, " +
                        $"TrackNo: {pl.TrackNo}, TrackTitle: {pl.TrackName}");
                }

                Console.ReadKey();
            }
        }
    }
    ```

    결과  
    ![13_07_NewJoinExam2Result.png](image/13/13_08_NewJoinExam3Result.png)  
    
    ```note
    메쏘드 구문보다 쿼리 구문이 이해하기가 훨씬 좋다. 데이터소스가 2개보다 많아지면 이해가 점점 더 복잡해진다. 이럴때는 가능한 쿼리 구문이 좀더 사용하기 편하다.
    ```

    <br>

## <font color='dodgerblue' size="6">2) GroupJoin</font>     

- ### A. GroupJoin 이란?
    sql의 입장에서 보면 Join 한 뒤에 그룹핑하는 것을 Linq에서는 GroupJoin 하나로 수행할 수 있다.

    앞의 그루핑쪽에서 본 것처럼 그룹 조인은 결과를 두개의 계층으로 생성한다. 첫번째 계층은 집합화된 결과, 두번째는 원본데이터이다. 
    
    Join메소드처럼 GroupJoin도 두 가지 오버로드된 버전이 존재한다.

    ![13_10_GroupJoin.png](image/13/13_10_GroupJoin.png)  

    역시나 차이점은 두 번째 오버로드된 버전이 추가 IEqualityComparer를 사용한다는 것이다.

    1. 외부 데이터 소스
    2. 내부 데이터 소스
    3. 외부 키 선택기
    4. 내부 키 선택기
    5. 결과 선택기

- ### B.메서드 또는 쿼리 구문을 사용하는 내부 Join 예제
        
        1. 간단 예제(2개 테이블 그룹조인)
        2. 쿼리 구문 사용 예제(into 사용)
        3. 사용자 정의구문예제(조인 컬럼이 2개보다 많을때)

    **예제1: 간단예제**  
    Artist와 Album 샘플 데이터를 보면 Artist는 3명이지만 6번 Album이 존재하지 않는 4번 아티스트기록이 있다. inner join을 먼저 하기 때문에 6번 앨범은 결과에서 제외될것이다. 그 이후 그룹핑을 수행하게 된다.

    ```cs
    using System.Linq;
    using System;

    namespace LINQJoin
    {
        class Program
        {
            static void Main(string[] args)
            {
                //Linq GroupJoin Method
                var GroupJoinMS = Artist.GetAllArtists()
                    .GroupJoin(
                        Album.GetAllAlbums(),
                        art => art.ArtistId,
                        album => album.ArtistId,
                        (art, album) => new {art, album});

                foreach (var group in GroupJoinMS)
                {
                    Console.WriteLine($"Artist: {group.art.Name}");

                    foreach (var item in group.album)
                    {
                        Console.WriteLine($"  AlbumNo: {item.AlbumId}  Album: {item.Title}");
                    }

                    Console.WriteLine();
                }

                Console.ReadKey();
            }
        }
    }
    ```

    결과  
    ![13_11_GroupJoinExam1Result.png](image/13/13_11_GroupJoinExam1Result.png)  
    
    6번 앨범의 Artist ID가 4인데 4인 아티스트는 존재하지 않기 때문에 결과에서 제외되었다.

    **예제2: 쿼리구문예제**  
    쿼리 구문에는 GroupJoin 연산자가 존재하지 않기 때문에 "into" 와 함께 내부 조인을 사용해야 한다.

    ```cs
    using System.Linq;
    using System;

    namespace LINQJoin
    {
        class Program
        {
            static void Main(string[] args)
            {
                //Linq Query
                var GroupJoinQS = from art in Artist.GetAllArtists()
                                join album in Album.GetAllAlbums()
                                    on art.ArtistId equals album.ArtistId
                                into ArtistGroups                       // Grouping 결과를 임시저장소에 저장
                                select new { art, ArtistGroups };     // Grouping 결과인 ArtistGroups 사용에 주의

                foreach (var group in GroupJoinQS)
                {
                    Console.WriteLine($"Artist: {group.art.Name}");

                    foreach (var item in group.ArtistGroups)
                    {
                        Console.WriteLine($"  AlbumNo: {item.AlbumId}  Album: {item.Title}");
                    }

                    Console.WriteLine();
                }

                Console.ReadKey();
            }
        }
    }
    ```


    **예제3: 사용자 정의구문예제**  
    아래 예와 같이 사용자 정의 이름을 지정할 수도 있습니다.

    ```cs
    using System.Linq;
    using System;

    namespace LINQJoin
    {
        class Program
        {
            static void Main(string[] args)
            {
                //Using Method Syntax
                var GroupJoinMS = Artist.GetAllArtists()
                .GroupJoin(
                    Album.GetAllAlbums(),
                    art => art.ArtistId,
                    album => album.ArtistId,
                    (art, album) => new
                    {
                        Artists = art,
                        Albums = album
                    }
                );

                //Linq Query
                var GroupJoinQS = from art in Artist.GetAllArtists()
                                join album in Album.GetAllAlbums()
                                    on art.ArtistId equals album.ArtistId
                                into ArtistGroups
                                select new 
                                {
                                    Artists = art,
                                    Albums = ArtistGroups
                                };

                foreach (var group in GroupJoinQS)
                {
                    Console.WriteLine($"Artist: {group.Artists.Name}");

                    foreach (var item in group.Albums)
                    {
                        Console.WriteLine($"  AlbumNo: {item.AlbumId}  Album: {item.Title}");
                    }

                    Console.WriteLine();
                }

                Console.ReadKey();
            }
        }
    }
    ```

    <br>

## <font color='dodgerblue' size="6">3) Left Join</font>     

- ### A. Left Join
    sql의 left join처럼 두 데이터 소스에서 왼쪽에 위치한 데이터 위주로 결과를 만들어낸다. 왼쪽 데이터가 오른쪽에 없으면 null로 표시된다.  

    ![13_12_LeftJoin.png](image/13/13_12_LeftJoin.png)  

    C#에서 Left Join을 구현하려면 DefaultIfEmpty() 메서드 와 함께 INTO 키워드를 사용해야 합니다.

- ### B.메서드 또는 쿼리 구문을 사용하는 Left Join 예제
        
        1. 쿼리 구문을 사용한 Left Outer Join 예제
        2. 메서드 구문을 사용한 Left Outer Join 예제
        3. 사용자 정의 속성 예제(컬럼 지정)    

    **예제1: 쿼리 구문 간단 예제**  
    쿼리 구문을 사용하여 왼쪽 외부 조인을 수행하려면 그룹 조인 결과에 대해 DefaultIfEmpty() 메서드를 호출해야 합니다. Linq에서 왼쪽 외부 조인을 구현하는 단계별 절차를 살펴보겠습니다.

    1단계  
    GroupJoin을 이용하여 내부 조인 수행. 
    
    ```cs
    from album in Album.GetAllAlbums()
    join tr in Track.GetAllTracks() 
        on album.AlbumId equals tr.AlbumId
    into AlbumTrackGroup         
    ```

    2단계  
    왼쪽의 데이터소스인 Album의 각 요소를 두번째인 Track에 일치하는 데이터가 없는지와 상관없이 결과 집합에 포함한다. 이러기 위해서는 일치하는 요소의 각 시퀀스에 대해 DefaultEmpty() 메서드를 호출해야 한다.

    이 예에서는 일치하는 Track의 각 요소에 대해 DefaultEmpty() 메서드를 호출한다. 이렇게 함으로써 일치하지 않고 비어 있는 경우 단일 기본값을 포함하는 컬렉션을 반환하여 각 Album 개체가 결과에 표시돠도록 한다. 아래처럼

    ```cs
    from tr in Track.GetAllTracks().DefaultEmpty() 
    ```

    ```note
    참조 유형의 기본값은 null입니다. 따라서 Track 컬렉션의 각 요소에 액세스하기 전에 null 참조를 확인해야 합니다.
    ```

    ```cs
    using System.Linq;
    using System;

    namespace LINQJoin
    {
        class Program
        {
            static void Main(string[] args)
            {
                var QSOuterJoin =
                                from album in Album.GetAllAlbums()
                                join tr in Track.GetAllTracks()
                                    on album.AlbumId equals tr.AlbumId
                                into AlbumTrackGroup
                                from track in AlbumTrackGroup.DefaultIfEmpty()
                                select new { album, track };

                foreach (var item in QSOuterJoin)
                {
                    Console.WriteLine($"Album: {item.album.Title, -25}, TrackTitle: {item.track?.Name}");
                }

                Console.ReadKey();
            }
        }
    }
    ```

    결과  
    ![13_13_LeftJoinExam1Result.png](image/13/13_13_LeftJoinExam1Result.png) 
    
    Let There Be Rock 앨범의 트랙정보가 없기 때문에 빈결과로 나온다. 위에서는 빨간색. 

    **예제2: 메서드 구문 간단 예제**  
    메서드 구문을 사용하여 Linq에서 왼쪽 외부 조인을 구현하려면 SelectMany() 및 DefaultIfEmpty() 메서드와 함께 GroupJoin() 메서드를 사용해야 합니다. 따라서 다음과 같이 메서드 구문을 사용하여 이전 예제를 다시 작성해 보겠습니다.

    ```cs
    using System.Linq;
    using System;

    namespace LINQJoin
    {
        class Program
        {
            static void Main(string[] args)
            {
                var MSOuterJOIN = Album.GetAllAlbums()
                                .GroupJoin(
                                        Track.GetAllTracks(),
                                        album => album.AlbumId,
                                        tr => tr.AlbumId,
                                        (album, tr) => new { album, tr }
                                )
                                .SelectMany(
                                        x => x.tr.DefaultIfEmpty(),
                                        (albumInfo, trackInfo) => new { albumInfo, trackInfo }
                                );

                foreach (var item in MSOuterJOIN)
                {
                    Console.WriteLine($"Album: { item.albumInfo.album.Title, -25 }, TrackTitle: { item.trackInfo?.Name }");
                }

                Console.ReadKey();
            }
        }
    }
    ```

    이전 예제와 동일한 출력을 제공합니다. 방법 구문보다 쿼리 구문을 사용하여 간단하고 이해하기 쉽기 때문에 Linq에서 왼쪽 외부 조인을 수행하는 것이 항상 더 좋다고 생각합니다.

    **예제3: 사용자 정의 속성이 있는 익명 유형**  

    ```cs
    using System.Linq;
    using System;

    namespace LINQJoin
    {
        class Program
        {
            static void Main(string[] args)
            {
                //Using Method Syntax
                var MSOuterJOIN = Album.GetAllAlbums()
                                .GroupJoin(
                                    Track.GetAllTracks(),
                                    album => album.AlbumId,
                                    tr => tr.AlbumId,
                                    (album, tr) => new { album, tr }
                                )
                                .SelectMany(
                                    x => x.tr.DefaultIfEmpty(),
                                    (albumInfo, trackInfo) => new
                                    {
                                        AlbumTitle = albumInfo.album.Title,
                                        TrackName = trackInfo == null ? "N/A" : trackInfo.Name
                                    }
                                );

                //Using Query Syntax
                var QSOuterJoin = 
                                from album in Album.GetAllAlbums()
                                join tr in Track.GetAllTracks()
                                    on album.AlbumId equals tr.AlbumId
                                into AlbumTrackGroup
                                from track in AlbumTrackGroup.DefaultIfEmpty()
                                select new
                                {
                                    AlbumTitle = album.Title,
                                    TrackName = track == null ? "N/A" : track.Name
                                };


                foreach (var item in MSOuterJOIN)
                {
                    Console.WriteLine($"Album : {item.AlbumTitle,-25}, TrackTitle : {item.TrackName} ");
                }

                Console.ReadKey();
            }
        }
    }
    ```

    ```note
    Right Outer Join 수행하려면 데이터 소스를 교환하기만 하면 된다.
    ```

    결과  
    ![13_14_LeftJoinExam3Result.png](image/13/13_14_LeftJoinExam3Result.png)  

    Track에 일치하는 데이터가 없어서 null이었는데 **N/A**라는 글자로 대체하였다.

    <br>
    
## <font color='dodgerblue' size="6">4) Cross Join</font>     
샘플 데이터
    
```cs
using System.Collections.Generic;

namespace LINQJoin
{
    public class A
    {
        public int ID { get; set; }
        public string AName { get; set; }
        public static List<A> GetAll_A()
        {
            return new List<A>()
            {
                new A { ID = 1, AName = "A_1"},
                new A { ID = 2, AName = "A_2"},
                new A { ID = 3, AName = "A_3"},
                new A { ID = 4, AName = "A_4"},
                new A { ID = 5, AName = "A_5"}
            };
        }
    }

    public class B
    {
        public int ID { get; set; }
        public string BName { get; set; }

        public static List<B> GetAll_B()
        {
            return new List<B>()
            {
                new B { ID = 1, BName = "B_1"},
                new B { ID = 2, BName = "B_2"},
                new B { ID = 3, BName = "B_3"}
            };
        }
    }
}
```

- ### A. Cross Join
    Linq Cross Join을 사용하여 두 개의 데이터 소스(또는 두 개의 컬렉션을 결합할 수 있음)를 결합할 때 첫 번째 데이터 소스(즉, 첫 번째 컬렉션)의 각 요소는 두 번째 데이터 소스(즉, 두 번째 컬렉션)의 모든 요소와 매핑됩니다. 따라서 간단히 말해서 교차 조인은 조인과 관련된 컬렉션 또는 데이터 소스의 데카르트 곱을 생성한다고 말할 수 있습니다.

    교차 조인에서는 조인 키를 지정하는 데 사용되는 "on" 키워드가 필요하지 않으므로 공통 키나 속성이 필요하지 않습니다. 또한 데이터 필터링이 없습니다. 따라서 결과 시퀀스의 총 요소 수는 조인에 관련된 두 데이터 소스의 곱이 됩니다. 첫 번째 데이터 소스가 5개의 요소를 포함하고 두 번째 데이터 소스가 3개의 요소를 포함하는 경우 결과 시퀀스에는 (5*3) 15개의 요소가 포함됩니다.

    <br>
- ### B.메서드 또는 쿼리 구문을 사용하는 Cross Join 예제

        1. Query 구문 Cross Join 예제
        2. 메서드 구문 Cross Join 예제

    <br>
    **예제1: Query 구문 Cross Join**  
    ```cs
    using System.Linq;
    using System;

    namespace LINQJoin
    {
        class Program
        {
            static void Main(string[] args)
            {
                var CrossJoinResult =
                                from a1 in A.GetAll_A()
                                from b1 in B.GetAll_B()
                                select new
                                {
                                    A1_Name = a1.AName,
                                    B1_Name = b1.BName
                                };

                foreach (var item in CrossJoinResult)
                {
                    Console.WriteLine($"AName: {item.A1_Name},    BName: {item.B1_Name}");
                }

                Console.ReadKey();
            }
        }
    }
    ```

    <div style='background-color:#fff5b1;border:1px solid black;width:300px;font-size:13px;padding:0.5em'>
    AName : A_1, &nbsp;&nbsp;&nbsp;BName: B_1<br>
    AName : A_1, &nbsp;&nbsp;&nbsp;BName: B_2<br>
    AName : A_1, &nbsp;&nbsp;&nbsp;BName: B_3<br>
    AName : A_2, &nbsp;&nbsp;&nbsp;BName: B_1<br>
    AName : A_2, &nbsp;&nbsp;&nbsp;BName: B_2<br>
    AName : A_2, &nbsp;&nbsp;&nbsp;BName: B_3<br>
    AName : A_3, &nbsp;&nbsp;&nbsp;BName: B_1<br>
    AName : A_3, &nbsp;&nbsp;&nbsp;BName: B_2<br>
    AName : A_3, &nbsp;&nbsp;&nbsp;BName: B_3<br>
    AName : A_4, &nbsp;&nbsp;&nbsp;BName: B_1<br>
    AName : A_4, &nbsp;&nbsp;&nbsp;BName: B_2<br>
    AName : A_4, &nbsp;&nbsp;&nbsp;BName: B_3<br>
    AName : A_5, &nbsp;&nbsp;&nbsp;BName: B_1<br>
    AName : A_5, &nbsp;&nbsp;&nbsp;BName: B_2<br>
    AName : A_5, &nbsp;&nbsp;&nbsp;BName: B_3<br>
    </div>

    A 컬렉션에는 5개의 데이터가 있고 B 컬렉션에는 3개가 있다. 결과 집합에는 15개의 요소, 즉 조인에 관련된 요소의 데카르트 곱의 결과가 된다.

    <br>
    **예제2: 메서드 구문 Cross Join**  
    ```cs
    using System.Linq;
    using System;

    namespace LINQJoin
    {
        class Program
        {
            static void Main(string[] args)
            {                                
                //Cross Join using SelectMany Method
                var CrossJoinResult = A.GetAll_A()
                                        .SelectMany(sub => B.GetAll_B(),
                                        (a1, b1) => new
                                        {
                                            A1_Name = a1.AName,
                                            B1_Name = b1.BName
                                        });

                //Cross Join using Join Method
                var CrossJoinResult2 = A.GetAll_A()
                                        .Join(B.GetAll_B(),
                                            a1 => true,
                                            b1 => true,
                                            (a1, b1) => new
                                            {
                                                A1_Name = a1.AName,
                                                B1_Name = b1.BName
                                            }
                                        );

                foreach (var item in CrossJoinResult)
                {
                    Console.WriteLine($"AName: {item.A1_Name},    BName: {item.B1_Name}");
                }

                Console.ReadKey();
            }
        }
    }
    ```

    이전 예제와 동일한 결과를 제공합니다.








