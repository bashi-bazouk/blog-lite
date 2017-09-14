## JSONParser.mly
```
/* Templated from https://caml.inria.fr/pub/docs/manual-ocaml/lexyacc.html */
%{

type json =
    Const of string
  | Array of json list 
  | Dict of (string * json) list 

%}

%token ARRAY_OPEN
%token ARRAY_CLOSE
%token DICT_OPEN
%token DICT_CLOSE
%token COMMA
%token <string> CONST
%token <string> LABEL

%start json
%type <json> json

%%
json:
    CONST                        { Const($1) }
  | ARRAY_OPEN ARRAY_CLOSE       { Array([]) }
  | ARRAY_OPEN items ARRAY_CLOSE { Array($2) }
  | DICT_OPEN DICT_CLOSE         { Dict([]) }
  | DICT_OPEN entries DICT_CLOSE { Dict($2) }
items:
    json             { [$1] }
  | json COMMA items { $1::$3 }
entries:
    LABEL json               { [($1, $2)] }
  | LABEL json COMMA entries { ($1, $2)::$4 }
;

%%

```

## JSONLexer.mll
```
(* Templated from https://caml.inria.fr/pub/docs/manual-ocaml/lexyacc.html *)

{ 
  open JSONParser
}

let s = [' ' '\t' '\n' '\r']
let string = '"' ([^ '\\' '"'] | ("\\" _))* '"'
let number = ['0'-'9']+ ('.' ['0'-'9']*)?
let const = string | number
let label = (string as l) s* ':'

rule tokenize = parse
    s+         { tokenize lexbuf }
  | '['        { ARRAY_OPEN }
  | ']'        { ARRAY_CLOSE }
  | '{'        { DICT_OPEN }
  | '}'        { DICT_CLOSE }
  | ','        { COMMA }
  | const as c { CONST(c) }
  | label      { LABEL(l) }

```

## JSON.ml
```ocaml
type json =
    Const of string
  | Array of json list
  | Dict of (string * json) list

let json_of_lexbuf lexbuf =
  let rec json_of_parser_json = function
      JSONParser.Const(c) -> Const(c)
    | JSONParser.Array(items) ->
      Array(List.map json_of_parser_json items)
    | JSONParser.Dict(entries) ->
      Dict(List.map (fun (s,e) -> (s, json_of_parser_json e)) entries) in
  json_of_parser_json (JSONParser.json JSONLexer.tokenize lexbuf)

let json_of_channel c =
  json_of_lexbuf (Lexing.from_channel c)

let json_of_string s =
  json_of_lexbuf (Lexing.from_string s)

let json_of_function f =
  json_of_lexbuf (Lexing.from_function f)

let rec string_of_json = function
    Const s -> s
  | Array([]) -> "[]"
  | Array(first_json::jsons) ->
      let string_of_jsons =
        List.fold_left 
          (fun s json -> s ^ "," ^ (string_of_json json)) 
          (string_of_json first_json) in
      "[" ^ (string_of_jsons jsons) ^ "]"
  | Dict([]) -> "{}"
  | Dict((first_label, first_json)::labelled_jsons) ->
      let string_of_labelled_jsons =
        List.fold_left 
          (fun s (label, json) -> s ^ "," ^ label ^ ":" ^ (string_of_json json))
          (first_label ^ ":" ^ (string_of_json first_json)) in
      "{" ^ (string_of_labelled_jsons labelled_jsons) ^ "}"
```

## Makefile
```make
JSONParser.ml: JSONParser.mly
	ocamlyacc JSONParser.mly
	rm JSONParser.mli

JSONParser.mli: JSONParser.ml
	ocamlc -i JSONParser.ml > JSONParser.mli

JSONParser.cmi: JSONParser.mli
	ocamlc -c JSONParser.mli

JSONParser.cmo: JSONParser.cmi JSONParser.ml
	ocamlc -c JSONParser.ml

JSONLexer.ml: JSONLexer.mll JSONParser.mli
	ocamllex JSONLexer.mll

JSONLexer.cmo: JSONParser.cmo JSONLexer.ml
	ocamlc -c JSONParser.cmo JSONLexer.ml

JSON.cma: JSONLexer.cmo JSONParser.cmo JSON.ml
	ocamlc -a -o JSON.cma JSONLexer.cmo JSONParser.cmo JSON.ml


.PHONY: clean

clean:
	-rm *.mli *.cm* JSONLexer.ml JSONParser.ml
```
