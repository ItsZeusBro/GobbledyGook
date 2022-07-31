# GobbledyGook

Gobbledy gook is a subset of schema that is sufficiently generalizable to the extent that we can apply the same method to it and always be doing something orderly, predictable, and useful.


## Itercursion
One of the methods used by Gobbledy Gook is something I call "intercursion". It's basically the same logic recursion and iteration use, without actually calling the function recursively, and without incrementing any index value. It recurses in place, and iterates in place, and because of that it can only be described as an iterator that removes objects using some sort of next() function. But you can virtualize the 'removal' process by holding some sort of data structure, making it act more like recursion.

Itercursion uses a sentinal value, and a container reference stack


        //                                                                                                                |
        //                                                                                                                |
        //                                                                                                                v  <---sentinel recognition
        //  [                                                                                                             ]  
        //      {                   isStr:                  },  {                    isArr:                             }
        //          [                                  ]           [                                               ]                  
        //              {   isStr:  },  {   isArr:  }                   {   isStr:  },    {   isArr:,   isInt: }                     
        //                  [   ]          [   ]                          [   ]            [   ]    [   ]                        
        //                    {}             {}                             {}              {}        {}                       
        
        
        
        //
        //                                                                             |                                 
        //                                                                             |                                 
        //  [                                                                          v                                  ]   
        //      {                   isStr:                  },  {                    isArr:                             }  <---update sentinel value
        //          [                                  ]           [                                               ]                  
        //              {   isStr:  },  {   isArr:  }                   {   isStr:  },    {   isArr:,   isInt: }  
        //                  [   ]          [   ]                          [   ]            [   ]    [   ]                     
        //                    {}             {}                             {}               {}       {}                        
        //
        
        
        
        //                                                                                                          |
        //  [                                                                                                       |      ]   
        //      {                   isStr:                  },                                                      v   <---sentinel consumes object
        //          [                                  ]           [                                                ]                  
        //              {   isStr:  },  {   isArr:  }                   {   isStr:  },    {   isArr:,   isInt: }  
        //                  [   ]          [   ]                          [   ]            [   ]    [   ]                     
        //                    {}              {}                           {}                {}      {}                        
        //



        //                                                                                                          |
        //                                                                                                          |                                  
        //  [                                                                                                       v       ]   
        //      {                   isStr:                  },      [                                               ]    
        //          [                                  ]              {   isStr:  },    {   isArr:,   isInt:   }          ^    
        //              {   isStr:  },  {   isArr:  }                      [   ]            [   ]     [   ]               | collapse
        //                  [   ]          [   ]                            {}                {}        {}                |
        //                    {}              {}                                                           
        //
        //                                                                                                          
        
        
        
        //                                                                                                     |                                           
        //  [                                                                                                  |             ]   
        //      {                   isStr:                  },      [                                          v    ]     <---sentinel recognition 
        //          [                                  ]              {   isStr:  },    {   isArr:,   isInt:   }              
        //              {   isStr:  },  {   isArr:  }                      [   ]             [   ]    [   ]             
        //                  [   ]          [   ]                            {}                {}        {}               
        //                    {}              {}                                                           
        //


        
        //                                                                                              |                                              
        //  [                                                                                           |                   ]   
        //      {                   isStr:                  },      [                                   v           ]     <---update sentinel value  
        //          [                                  ]              {   isStr:  },    {   isArr:,   isInt:   }              
        //              {   isStr:  },  {   isArr:  }                       [   ]            [   ]    [   ]             
        //                  [   ]          [   ]                            {}                {}        {}               
        //                    {}              {}                                                           
        //



        //                                                                                                                                                 
        //  [                                                                                                             ]   
        //      {                   isStr:                  },      [                                    |          ]       
        //          [                                  ]               {   isStr:  },    {   isArr:      |     }        <---Sentinel consumes object 
        //              {   isStr:  },  {   isArr:  }                       [   ]            [   ]    [  v ]                
        //                  [   ]          [   ]                            {}                {}        {}                        
        //                    {}              {}                                                           
        //

        //                                                                                                                                                 
        //  [                                                                                                             ]   
        //      {                   isStr:                  },      [                                    |          ]       
        //          [                                  ]               {   isStr:  },    {   isArr:      |     }        <---Delete object 
        //              {   isStr:  },  {   isArr:  }                       [   ]            [   ]    [  v ]                
        //                  [   ]          [   ]                            {}                {}                                
        //                    {}              {}                                                           
        //



        //                                                                                                                                                 
        //  [                                                                                               |               ]   
        //      {                   isStr:                  },      [                                       |         ]       
        //          [                                  ]               {   isStr:  },    {   isArr:         v  }        
        //              {   isStr:  },  {   isArr:  }                       [   ]            [   ]     [    ]                         
        //                  [   ]          [   ]                             {}               {}             <--- Sentinel pops stack and sees empty array       
        //                    {}             {}                                                           
        //



        //                                                                                                      |                                         
        //  [                                                                                                   |           ]   
        //      {                   isStr:                  },      [                                           v      ]       
        //          [                                  ]               {   isStr:  },    {        isArr:        }        
        //              {   isStr:  },  {   isArr:  }                       [   ]                  [   ]               <--- delete empty array             
        //                  [   ]          [   ]                             {}                     {}                               
        //                    {}             {}                                                           
        //



        //                                                                                                       |                                         
        //  [                                                                                                    |          ]   
        //      {                   isStr:                  },      [                                            v    ]       
        //          [                                  ]               {   isStr:  },     {       isArr:         }    <---nothing to collapse
        //              {   isStr:  },  {   isArr:  }                       [   ]                  [  ]                          
        //                  [   ]          [   ]                             {}                     {}                               
        //                    {}              {}                                                           
        //



        //                                                                                           |                                           
        //  [                                                                                        |                     ]   
        //      {                   isStr:                  },      [                                v                 ]       
        //          [                                  ]               {   isStr:  },     {       isArr:         }    <---sentinel updates value
        //              {   isStr:  },  {   isArr:  }                       [   ]                  [  ]                          
        //                  [   ]          [   ]                             {}                     {}                               
        //                    {}              {}                                                           
        //



        //                                                                                                                                                 
        //  [                                                                                         |                      ]   
        //      {                   isStr:                  },      [                                 |               ]      
        //          [                                 ]                       {   isStr:  },        [ v  ]      <---Sentinel consumes object  
        //              {   isStr:  },  {   isArr:  }                             [   ]               {}               
        //                  [   ]          [   ]                                   {}                                        
        //                    {}             {}                                                           
        //



        //                                                                                               |                                                 
        //  [                                                                                            |                   ]   
        //      {                   isStr:                  },      [                                    v           ]      
        //          [                                 ]                       {   isStr:  },         [   ]                           
        //              {   isStr:  },  {   isArr:  }                             [   ]                   <---Sentinel delete empty object inside array
        //                  [   ]          [   ]                                   {}                                        
        //                    {}              {}                                                           
        //



        //                                                                                                           |
        //                                                                                                           |                                     
        //  [                                                                                                        v      ]   
        //      {                   isStr:                  },      [                                                ]      
        //          [                                 ]                       {   isStr:  },                         <---delete empty array         
        //              {   isStr:  },  {   isArr:  }                             [   ]                                    
        //                  [   ]          [   ]                                   {}                                        
        //                    {}              {}                                                           
       
       
       
        //                                                                                                           |
        //                                                                                                           |                                     
        //  [                                                                                                        v      ]   
        //      {                   isStr:                  },      [                                                ]      
        //          [                                 ]                       {   isStr:  },                         <---nothing to collapse
        //              {   isStr:  },  {   isArr:  }                             [   ]                                    
        //                  [   ]          [   ]                                   {}                                        
        //                    {}              {}                                                           
        //
        
        
        
        //                                                                    |                                                                         
        //  [                                                                 |                                             ]   
        //      {                   isStr:                  },      [         v                                       ]<---update sentinel      
        //          [                                  ]               {   isStr:  }                                   
        //              {   isStr:  },  {   isArr:  }                       [   ]                    
        //                  [   ]          [   ]                             {}                                        
        //                    {}             {}                                                           
        //



        //                                                                                                                                                 
        //  [                                                                                                              ]   
        //      {                   isStr:                  },      [         |                                       ]       
        //          [                                  ]                      |                                       <---Sentinel consumes object
        //              {   isStr:  },  {   isArr:  }                       [ v ]                    
        //                  [   ]          [   ]                             {}                                        
        //                   {}             {}                                                           
        //



        //                                                                    |                                                                           
        //  [                                                                 |                                            ]   
        //      {                   isStr:                  },      [         v                                       ]       
        //          [                                  ]                 [    ]                                                                 
        //              {   isStr:  },  {   isArr:  }                           <---delete empty object                                              
        //                  [   ]          [   ]                                                                    
        //                    {}              {}                                                           
        //
        
        
        
        //                                                                                                            |
        //                                                                                                            |                                   
        //  [                                                                                                         v   ]   
        //      {                   isStr:                  },      [                                                 ]       
        //          [                                  ]                                                              <---delete empty array      
        //              {   isStr:  },  {   isArr:  }                                                                           
        //                  [   ]          [   ]                                                                    
        //                    {}              {}                                                           
        //


        //                                                                                                              |
        //                                                                                                              |
        //                                                                                                              v                                 
        //  [                                                                                                           ]   
        //      {                   isStr:                  }                                                           <---delete empty array
        //          [                                  ]                                                                                      
        //              {   isStr:  },  {   isArr:  }                                                                           
        //                  [   ]          [   ]                                                                    
        //                    {}              {}                                                           
        //


        //                                                  |
        //                                                  |                                                                                             
        //  [                                               v                                                            ]   
        //      {                   isStr:                  }                                                           <---update sentinel value
        //          [                                  ]                                                                                      
        //              {   isStr:  },  {   isArr:  }                                                                           
        //                  [   ]          [   ]                                                                    
        //                    {}              {}                                                           
        //
