import 'dart:async';
import 'dart:convert';
import 'package:http/http.dart' as http;
import 'package:m_tryout/db/databasehelper.dart';
import 'package:m_tryout/db/penggunamodel.dart';
import 'package:m_tryout/db/tryoutprocess.dart';
import '../berita/Detailberita.dart';
import 'package:flutter/material.dart';






class Nilai extends StatefulWidget {
  @override
  _NilaiState createState() => new _NilaiState();
}

class _NilaiState extends State<Nilai> {
  dynamic subyek;
  bool _aktif = false;
  bool _sudahujian = false;
  TryoutprocessModel _tryoutprocessModel;

   Future<List> cekMateri()async{
     
      PenggunaModel pm = new PenggunaModel(DatabaseHelper.internal());
      List<Map> ret = await pm.read(limit:1,offset: 0);
      _sudahujian = false;
      print('cek materi penggunamodel ${ret.length}');

      if(ret.length > 0){
          pm = pm.fromMap(ret[0]);
          ret = await _tryoutprocessModel.read(where: 'tbl_register_id=? AND materi_id=? ',
                    whereArgs: [pm.id, subyek['id_materi']],
                    orderBy: 'dt_mulai DESC');

          print('cek materi tryoutprocess ${ret.length}');
          if(ret.length > 0){
              _tryoutprocessModel = _tryoutprocessModel.fromMap(ret[0]);
                print('isi tryoutprocessmodel isopen = ${_tryoutprocessModel.isopen}');

          }                  
     } 

     setState(() { });

  }
  @override
  Widget build(BuildContext context) {
    return new Scaffold(
      appBar: new AppBar(
          title: new Text("Data Rekap Nilai Ujian "),
          backgroundColor: Colors.teal,
         
        ),
      body: new FutureBuilder<List>(
        future: cekMateri(),
        builder: (context, snapshot) {
          if (snapshot.hasError) print(snapshot.error);

          return snapshot.hasData
              ? new ItemList(
                  list: snapshot.data,
                )
              : new Center(
                  child: new CircularProgressIndicator(),
                );
        },
      ),
    );
  }
}

class ItemList extends StatelessWidget {
  final List list;
  ItemList({this.list});

  @override
  Widget build(BuildContext context) {
    return new ListView.builder(
      itemCount: list == null ? 0 : list.length,
      itemBuilder: (context, i) {
        return new Container(
          padding: const EdgeInsets.all(4.0),
          child: new GestureDetector(
             child: new Card(
                  child: Column(
                   children: <Widget>[
                   new Padding(padding: const EdgeInsets.only(top: 15.0),),
              

                   new Center( 
                     child:new Column(
              children: <Widget>[

                new Container(
                  padding: new EdgeInsets.only(left:10.0),
                  child: new Row(
                    mainAxisAlignment: MainAxisAlignment.start,
                    crossAxisAlignment: CrossAxisAlignment.start,
                    mainAxisSize: MainAxisSize.max,
                    children: <Widget>[
                        
                         new Padding(padding: new EdgeInsets.only(top: 15.0),),
                         new Padding(padding: new EdgeInsets.only(left: 10.0),),
                      new Text("Kategori: ",
                          style: TextStyle(
                              fontSize: 20.0,
                              color: Colors.red,
                              fontWeight: FontWeight.bold)),
                                  
                              
                    
                    ],
                  ),
                  
                ),

                           new Column(
  
      children: <Widget>[
     new Row(
       children: <Widget>[
         
         new Padding(padding: EdgeInsets.all(15.0),),
      new InkWell(
                 
                     child: Column(
                        mainAxisSize: MainAxisSize.min,
                      crossAxisAlignment: CrossAxisAlignment.center,
                     children: <Widget>[
                     
                      new Padding(padding: EdgeInsets.all(5.0),),
                       Text("Pelajaran", style: TextStyle(fontSize: 20.0, color: Colors.brown[600],
                     fontWeight: FontWeight.bold,
                         fontFamily: 'Montserrat')),
                         ],
                     ),
                 
                  ),
                  new Padding(padding: EdgeInsets.all(30.0),),
                  new InkWell(
                 
                     child: Column(
                        mainAxisSize: MainAxisSize.min,
                      crossAxisAlignment: CrossAxisAlignment.center,
                     
                     ),
                 
                  ),
                   new Padding(padding: EdgeInsets.all(30.0),),
                  new InkWell(
                 
                     child: Column(
                        mainAxisSize: MainAxisSize.min,
                      crossAxisAlignment: CrossAxisAlignment.center,
                     children: <Widget>[
                          new Padding(padding: EdgeInsets.all(5.0),),
                          Text("Score", style: TextStyle(fontSize: 20.0, color: Colors.brown[600],
                     fontWeight: FontWeight.bold,
                         fontFamily: 'Montserrat')),
                         ],
                     ),
                 
                  )

       ],
        
     ),
      ],
      ),

                new Container(
                  padding: new EdgeInsets.all(10.0),
                  child: new Row(
                    
                    mainAxisAlignment: MainAxisAlignment.start,
                    crossAxisAlignment: CrossAxisAlignment.start,
                    mainAxisSize: MainAxisSize.max,
                    
                    children: <Widget>[

                         new Padding(padding: new EdgeInsets.only(top: 15.0),),
                         new Padding(padding: new EdgeInsets.only(right: 50.0),),
                         
                      new Text("0",
                          style: TextStyle(
                              fontSize: 20.0,
                              color: Colors.teal[900],
                              fontWeight: FontWeight.bold)),
                            new Padding(padding: new EdgeInsets.only(left: 100.0),),
                            


                     new Padding(padding: new EdgeInsets.only(left: 90.0),),
                              new Text("0",
                          style: TextStyle(
                              fontSize: 20.0,
                              color: Colors.teal[900],
                              fontWeight: FontWeight.bold)),
                            
                    
                    ],
                  ),
                )
              ],
          
            ),
            
                  ),
              
        ]
                  ),
                    
                  
                ),
          ),
        );
      },
    );
  }
}




