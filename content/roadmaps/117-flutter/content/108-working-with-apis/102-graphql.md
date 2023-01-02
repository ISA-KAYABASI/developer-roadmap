# Graphql

To use GraphQL in a Flutter app, you can use the graphql_flutter package, which provides a GraphQL client for Flutter that can be used to execute GraphQL queries and mutations.

Here's an example of how to use the graphql_flutter package to execute a GraphQL query and display the results in a Flutter app:

Copy code
import 'package:flutter/material.dart';
import 'package:graphql_flutter/graphql_flutter.dart';

void main() {
  runApp(
    GraphQLProvider(
      client: yourClient,
      child: CacheProvider(
        child: MyApp(),
      ),
    ),
  );
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    final HttpLink httpLink = HttpLink(
      uri: 'https://your-api.com/graphql',
    );

    final ValueNotifier<GraphQLClient> client = ValueNotifier(
      GraphQLClient(
        cache: InMemoryCache(),
        link: httpLink,
      ),
    );

    return MaterialApp(
      home: Scaffold(
        appBar: AppBar(
          title: Text('GraphQL Flutter Example'),
        ),
        body: Query(
          options: QueryOptions(
            document: yourQuery,
          ),
          builder: (QueryResult result, { VoidCallback refetch }) {
            if (result.errors != null) {
              return Text(result.errors.toString());
            }

            if (result.loading) {
              return Text('Loading');
            }

            return Text(result.data.toString());
          },
        ),
      ),
    );
  }
}
In this example, we're using the GraphQLProvider widget to provide the GraphQLClient to the rest of the app. We're also using the Query widget to execute a GraphQL query and display the results.

To execute a GraphQL mutation, you can use the Mutation widget in a similar way.

Copy code
Mutation(
  options: MutationOptions(
    document: yourMutation,
  ),
  builder: (
    RunMutation runMutation,
    QueryResult result,
  ) {
    return FlatButton(
      onPressed: () async {
        final MutationOptions options = MutationOptions(
          document: yourMutation,
        );

        final QueryResult result = await runMutation(options);
      },
      child: Text('Run Mutation'),
    );
  },
)
